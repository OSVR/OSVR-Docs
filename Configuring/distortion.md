# OSVR Distortion Correction

This document describes the OSVR distortion correction, which happens after the display is rendered onto a planar rectangle. See the [projection and view matrices document](./projectionAndViewMatrices.md) for how to handle this phase of rendering.

## Overview

The basic function of distortion correction is to gather to physical display pixels viewed through a lens that causes distortion (or displayed on a non-planar screen) from the appropriate location on a canonical rectangular, planar screen.  An overfilled version of this rectangular, planar screen is produced by the projection and view matrices.  To avoid undefined visible regions, the canonical screen together with the overfill factor must be sufficient to cover all points that the undistortion map pulls into view.

Conceptually (1) The projection and view transforms take points in 3D model space and project them onto a canonical rectangular and planar screen, and (2) distortion correction adjusts the resulting image to undo the nonlinear effects of lenses or curved screens used to view it, pulling each point from its canonical location back into its physical location.  (The canonical rectangular, planar projection screen to be used is also determined as part of the distortion-correction process.)

## Approach

There are many potential classes of distortion, including those introduced by lenses and those introduced by display on a non-planar surface.  Within its rendering pipeline, OSVR handles the general case using a distortion mesh; its configuration files also include the ability to specify per-color radial distortion parameters.  Two specific cases are discussed below and then the general solution is described.

### Curved screen

The image below shows an example of a curved display (like the currently-available OLED TVs) viewed without a lens from a viewpoint in the middle of the screen along the screen's normal at that location.  The left part of the image shows a top-down 1D view of the scene and the right side shows a first-person view of the screen from the eye's point of view.  As with radially-distorting lenses, **distortion correction for a curved screen depends on the viewer's eye position**.  The more curved the screen, then more pronounced the effect.

![Cylindrical Screen](./images/cylindrical_screen.png)

In this figure, the blue screen is the (distorted) screen that would be seen and the green rectangle is one possible choice for an undistorted screen image.  We are free to choose any depth for the image of the screen so long as we adjust its projected size to match the extents seen in the 2D view -- it could be brought closer and scaled down or pushed back further and scaled up.  This particular image of the screen is smaller than the physical screen -- there are locations on the physical screen that are outside the virtual screen.

The image below shows the OSVR approach to undistorting the cylindrical projection.

![Cylindrical Undistortion](./images/undistortion.png)

**What is not done:** The right side of the image shows an approach that might be taken, but is not.  This is to pre-distort the original scene geometry by the inverse of the optical distortion that will be done by the cylindrical projection so that the resulting rendered image can be directly drawn on the (blue) screen and have the correct (green) projected image.  This requires applying an arbitrary nonlinear mapping to the geometry, which is not easily done and which would do piecewise-linear interpolation across triangles.  Another way to do this same pre-distortion would be to do a standard render pass and then do a second pass where the original rectangular texture generated in the first pass is rendered onto distorted geometry that projects onto the appropriate location on the screen (similar to the second rendering pass described in the approach below).  Either method produces an image that includes a non-linear warping of the geometry, resulting in an image that cannot be translated or rotated to handle the effects of head rotation ("time warp"), leading us to choose a different approach for the OSVR system.

**What is done:** The left side of the image shows the approach taken by OSVR, which is to determine what point on the virtual screen's image (green) corresponds to each point on the distorted real screen (blue).  During the final render pass, the texture coordinates for each point are adjusted to read each visible pixel from its corresponding location.  This undistortion can be done in the **vertex shader** by producing a dense mesh that has adjusted texture coordinates for each color or in the **pixel shader** either by applying a function to the texture coordinates or using a **texture map** to provide the new texture coordinate for each color to map to the proper location on the screen.

The initial implementation within the OSVR RenderManager uses the distortion-mesh approach.

### Per-color radial distortion

The image below shows an example of a viewing a rectangular screen (black) through a lens system that has radial distortion.  This particular lens has a different magnification for each wavelength of light, resulting in three different distorted images, one per primary color.

![Per-color radial distortion](./images/chromatic_radial_distortion.png)

This distortion happens in addition to the desired behavior of the lens, which is to magnify the physical display and to move its virtual image further from the eye so that the viewer can focus on it.  It is possible to make lens systems that are achromatic and produce the same per-color distortion, but it is also possible to correct for this chromatic distortion within the rendering system.

Here, the distortion depends on the angle of line from the viewer's pupil through a point on the physical screen compared to the ray from the viewer's pupil through the center of the lens.  Although the position of the virtual image of the screen for an ideal lens does not depend on the position of the user's eye (so long as the eye is in the exit pupil for the lens), **radial distortion depends on the location of the viewer's eye**.  This means that completely correcting for radial distortion requires taking into account the distance between the viewer's eyes as well as knowing the locations of the lenses with respect to the screen.

OSVR configuration files describe this distortion using a set of parameters:

* **Center of projection:** This provides the coordinates for the location on the virtual image of the screen where the ray from the center of the viewer's eye through the center of projection of the lens intersects it.  This is a fractional coordinate from 0-1 in each axis, with the lower-left corner of the screen being (0,0) and the upper-right being (1,1).

* **Distance scales:** Because distortion correction depends on the lens geometry and the screen geometry, and may not be directly related to the viewport size or aspect ratio (for lenses that expand more in one direction than the other), we need to allow the specification of not only the radial distortion polynomial coefficients (which scale powers of the distance from the center of projection to the point), but also the space in which this is measured.  We specify the space by telling the number of unit radii in the space the parameters are defined in lies across the texture coordinates, which range from 0 to 1.  This can be different for X and Y, as the viewport may be non-square and the lens system may make yet a different aspect ratio.  The D[0] component tells the width and D[1] tells the height.

* **Per-color coefficients:** There is a set of polynomial coefficients provided for each color.  The coefficients for specify the new radial displacement from the center of projection as a function that scales the original displacement.   The first coefficient in each polynomial is a constant factor (multiplied by offset^0, or 1), the second is the linear factor, the third is quadratic, and so forth.  There can be as many coefficients as desired.

The coefficients for R, G, and B; and the Distances for X and Y; and the center of projection may be specified in any consistent space that is desired (scaling all of them linearly will have no impact on the result), but lower-left corner of the space (as viewed on the screen) must be (0,0) and the far side of the pixels on the top and right are at the D-specified locations.

The parameters for each color specify the new radial displacement from the center of projection as a function of the original displacement.  In D-scaled space, this is:

    Offset = Orig - COP;              // Vector
    OffsetMag = sqrt(Offset.length() * Offset.length());  // Scalar
    NormOffset = Offset / OffsetMag;  // Vector
    Final = COP + (a0 + a1*OffsetMag + a2*OffsetMag*OffsetMag + ...)
        * NormOffset; // Position

Examples: (1) For a display 10 pixels wide by 8 pixels high that has square pixels whose center of projection is in the middle of the image, we would get: D = (10, 8); COP = (4.5, 3.5); parameters specified in pixel-unit offsets.  (2) For a display that is 6 units wide by 12 units high, but whose optics stretch the view horizontally to produce a square viewing image with pixels that are stretched in X, we could have: D = (12, 12); COP = (5.5, 5.5); parameters specified in vertical pixel-sized units **or** D = (6,6); COP = (2.5, 2.5); parameters specified in horizontal pixel-sized units.

### Overfill

There are some locations on the physical screen shown in the example above that do not correspond to any location on the image of the screen chosen above.  This means that there is no image to be moved to that location.  To avoid this, the virtual image of the screen (green) can be selected so that it completely surrounds the real image, providing images both where they will be visible and where they will not.  In this example, the screen is also moved back from the real screen, which will cause the distortion correction to depend more strongly on the eye position.  A better solution would place the virtual screen somewhere within the depths covered by the physical screen.

![Overfill](./images/overfill.png)

This overfill is also required for other distortions, including radially-symmetric distortions.  This is because any non-linear distortion will turn the rectangular boundary of the screen into a set of curves.

## Interaction with Time Warp

@todo

## Screen Used by Projection and Viewing

As described in the overview, the basic function of distortion correction is to move pixels viewed through a lens that causes distortion (or displayed on a non-planar screen) into the appropriate location such that they appear to be coming from a canonical rectangular screen, the image of which may only be partially visible.  This canonical image of a rectangular, planar screen is what is used by the projection and view matrices.

This means that the distortion correction is free to select any rectangle as the screen to be projected on, so long as it properly undistorts images rendered onto that rectangle.  Note that **OSVR RenderManager produces an overfilled viewport, enabling pixels to be pulled from outside the virtual screen rectangle**.  An optimal canonical rectangle should cover as much of the visible region on the physical display as possible to minimize required distortion (and thus overfill), and it should lie in depth within the convex hull of the virtual image of the real screen (to reduce shift in distortion as the eye rotates to look in different directions).

Distortion correction does pre-distortion using a look-up for every point in the physical display to determine what should be drawn there.  What should be drawn there is a point from an overfilled version of the canonical screen.  The final, pre-distorted image is provided to the scan-out circuitry and then viewed through whatever lens system on the surface of whatever shape the actual display is using.  The optics and screen itself undo this pre-distortion, mapping a subset of the overfilled canonical surface into the correct set of angles to be seen by viewer as an undistorted image.

## Example: Using a screen-to-angle table

Suppose that either through direct measurement with a camera or through ray-tracing in the optical design for a head-mounted display, you produce a mapping between physical points on the display screen and angles from the center of the eye, for a given IPD.  This mapping can be arbitrary, so long as it is a function (does not contain folds) and may come as a grid of points.  For this example, we will assume the angles are specified in degrees from views looking along the -Z axis in head space (straight forward) and the positions on the display are specified as distances in millimeters from the point on the display that corresponds to the point that would be see at angle (0,0).  Further, we assume that the focal distance to the visible screen is around 2 meters (some portions being closer, and some further).

(What follows here is a theoretical treatment, a program that implements this is described in the [AnglesToConfig program documentation](https://github.com/OSVR/distortionizer/blob/master/angles_to_config/doc/anglesToConfig.md)).

### Step 1: Determine canonical screen and overfill

This procedure is implemented in the **angles_to_config** directory in the OSVR Distortionizer project. (This procedure, and planar projection in general, will only work for displays whose monocular horizontal field of view is less than 180 degrees.)

We determine the four edges of the canonical screen and the required overfill in a way that can be expressed within the current OSVR RenderManager.  The normal to the screen must lie in the X-Z plane with the screen lying half above and half below the Y=0 axis.  It is specified with a horizontal and vertical field of view, a center of projection where the perpendicular line from the eye meets the screen, and a rotation around the eye around the Y axis.

The **eye-space location** of each point in the configuration file is computing using polar coordinates, using the 2-meter focus estimate as the radius for each point, the longitudinal angle (positive spin around the Y axis with 0 facing forward), and the latitudinal angle (positive up).

The **X screen-space extents** are defined by the lines perpendicular to the Y axis passing through:
* **left:** the point location whose reprojection into the Y=0 plane has the most-positive angle (note that this may not be the point with the largest longitudinal coordinate, because of the impact of changing latitude on X-Z position).
* **right:** the point location whose reprojection into the Y=0 plane has the most-negative angle (note that this may not be the point with the smallest longitudinal coordinate, because of the impact of changing latitude on X-Z position).

The **Y screen-space extents** are symmetric and correspond to the lines parallel to the screen X axis that are within the plane of the X line specifying the axis extents at the largest magnitude angle up or down from the horizontal.  We determine this by finding the point with the largest-magnitude Y value when it is projected into the plane of the screen as determined by the X screen-space extents.

Because the projections of all of the points in the set will lie within these screen-space extents, implying that no points from outside this region correspond to any point on the physical screen, the overfill factor for the screen can be 1 (there does not need to be additional overfill).

### Step 2: Specifying the screen in the config file

We convert these into the specifications used in the current OSVR config-file format.  (In the future, this will be replaced by specifying the head-space location of three of the four screen corners, as described in the projection and viewing document.)  The current description includes the specification of a horizontal and vertical field of view, a center of projection (which is the normalized location on the screen where the line through the eye point perpendicular to the screen pierces the screen) and the percent overlap.

![Determine Canonical Screen](./images/determine_canonical.png)

* **display/hmd/field_of_view/monocular_horizontal:** This is computed as if the screen is being viewed by an eyepoint located along the line perpendicular to the center of the screen.  We determine it using the half-screen width and the perpendicular distance from the origin to the plane of the screen.  The ratio of these specify the tangent of half of the horizontal field of view.
* **display/hmd/field_of_view/monocular_vertical:** This is computed as if the screen is being viewed by an eyepoint located along the line perpendicular to the center of the screen.  We determine it using the half-screen height and the perpendicular distance from the origin to the plane of the screen.  The ratio of these specify the tangent of half of the vertical field of view.
* **display/hmd/field_of_view/overlap_percent:** This is computed as if the screen is being viewed by an eyepoint located along the line perpendicular to the center of the screen and as if both eyes were co-located (IPD = 0).  (Note; the resulting viewing transform does not make this assumption, just the current algorithm to map from overlap_percent to angle.)
* **display/hmd/eyes[1]/center_proj_x:** This is computed as the fraction of the way from the left side of the screen to the right side where the line through the eye perpendicular to the screen crosses the screen.

### Step 3: Mapping from physical screen coordinates

Given points in the physical screen, the distortion map provides the coordinates of the corresponding point on the canonical rectangular screen (or perhaps outside the screen, in the overfill region or beyond).  This determines the appropriate point to display at this location on the screen.  This is done in two steps:
* **Step 3A:** Map from physical-display coordinate to angle using the provided table.
* **Step 3B:** Map from angle to virtual-screen coordinate by projecting the ray from the eye onto the plane of the canonical screen.  Then determine the screen-space X and Y coordinates (X = 0 at left and 1 at the right, Y = 0 at the bottom and 1 at the top).

Doing this mapping for points other than those specified in the table requires interpolation for display points between those specified and extrapolation for points outside their convex hull.

### Step 4: Producing distortion map in config file

The configuration file format allows the specification of a variety of distortions, identified by the **display/hmd/distortion/type** variable.  We'll be assuming that the red, green, and blue components of the distortion are all the same, so using the type "mono_point_samples".  This means that we need to specify just one distortion mesh, which maps from normalized (X,Y) coordinates in a the physical display ([0,0] at the lower-left corner, [1,1] at the upper right) into normalized coordinates in a canonical rectangle that has an overfill value of 1 (no additional overfill).  Values may map outside the range [0,0]-[1,1] due to the distortion; they will map initially into the overfill area and then eventually beyond if there is insufficient overfill.  For our approach above, none of the points will map outside this range, because all visible display points maps to locations that lie within the projected area of the canonical screen even with an overfill factor of 1.

We compute the input normalized coordinates for the mesh by normalizing the table's display coordinates to convert them from millimeters to screen fractions, subtracting the coordinates of the lower-left corner of the screen and dividing each axis by the screen dimension.  We compute the output coordinates as describe in Step 3.

We then store the unordered set of points into the **display/hmd/distortion/mono_point_samples** array, which has a vector of elements, each of which has two elements, the first of which is the 2D coordinates in normalized physical-screen coordinates and the second of which is the 2D coordinates in the canonical-screen coordinates.

An example output, which is a partial description of an HMD, follows.  It provides the identity mapping.

    {
      "display": {
        "hmd": {
          "distortion": {
            "type": "mono_point_samples",
            "mono_point_samples": [
              [ [0,0], [0,0] ],
              [ [1,0], [1,0] ],
              [ [0,1], [0,1] ],
              [ [1,1], [1,1] ]
            ]
          }
        }
      }
    }

The RenderManager uses this set of unordered point samples to compute a mesh by using a bilinear fit to the nearest 3 non-coplanar points to determine each of the coordinates for each point in space that must be sampled to produce a mesh with the specified number of points.

## Built-in Distortion Meshes
Distortion meshes for HDK 1.3 and HDK 2.0 have been built-in to RenderManager. They can be loaded by name in the "distortion" section of a display config. For example, to load the HDK 2.0 distortion mesh:
```json
"distortion": {
				"type": "mono_point_samples",
				"mono_point_samples_built_in": "OSVR_HDK_20_V1"
			},
```
Note that OSVR_HDK_20_V1 is only available in RenderManager v00_06_43-93-g3a45de8 or later.
For the HDK 1.3, the name of the mesh is "OSVR_HDK_13_V2" or "OSVR_HDK_13_V1". An example display descriptor with built-in mesh for HDK 1.3 can be found at https://github.com/OSVR/OSVR-Core/blob/master/apps/displays/OSVR_HDK_1_3_with_mesh.json.
