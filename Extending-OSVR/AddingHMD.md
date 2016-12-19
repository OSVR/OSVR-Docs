# Adding a New HMD to OSVR

## Introduction
OSVR creates an abstration layer for many types of devices, including HMDs. This means that the same application can run on different HMDs simply by changing the configuration file that the OSVR server processes when it launches. Adding your HMD to OSVR instantly allows your customers to enjoy the OSVR applications. This note explains the key steps in adding an HMD to OSVR. These are:

- Create a display descriptor. This specifies the key attributes of the HMD such as the field of view
- Provide information about distortion. OSVR can apply distortion correction for the HMD. This step is optional.
- Support direct rendering. When used with a supported graphics card, OSVR provides direct rendering support to HMDs and this section discusses how to add this capability to your HMD through OSVR
- Integrate tracking information. Often, HMDs also include peripherals such as an orientation tracker. OSVR passes this information along to applications so they can update the view of the virtual world.
- Verify your HMD is working with OSVR. Ensure that the display is free of distortion and that the tracking is working well.
- Publish your HMD plugin. Release your OSVR plugin to the world so developers and gamers can use your HMD with their OSVR software and games.

## Display integration
To start with OSVR, all you need to get a display going initially is just a "display descriptor" JSON file. You can start with a very simple one—the one for the [OSVR HDK 1.1](https://github.com/OSVR/OSVR-Core/blob/master/apps/displays/OSVR_HDK_1_1.json) is about as simple as they get—and modify it to suit your display.

- [Display descriptor documentation](https://github.com/OSVR/OSVR-JSON-Schemas/blob/master/doc/display_descriptor_schema_v1/Structure%20of%20JSON%20descriptor%20file%20for%20HMDs.md)
- [Display descriptor JSON schema](https://github.com/OSVR/OSVR-JSON-Schemas/blob/master/display_descriptor_schema_v1.json)
- [Existing display descriptors for additional reference](https://github.com/OSVR/OSVR-Core/tree/master/apps/displays)

The only fields you must modify are:

- The metadata fields - primarily for human consumption, though Render Manager also uses `vendor`:
	- `hmd.device.vendor`
	- `hmd.device.model`
	- `hmd.device.Version`
	- `hmd.device.Note`
- The basic optics:
	- `hmd.field_of_view.monocular_horizontal`
	- `hmd.field_of_view.monocular_vertical`
	- `hmd.field_of_view.overlap` if the screens are not aligned (gross generalization)
- The basic display control/input data:
	- In the resolution entry, the `width` and `height`, and, if yours isn't a single input horizontal side-by-side, `display_mode` and potentially `video_inputs`.

To use a display descriptor with an OSVR server config file, you add or edit the `"display": "???.json"` line. See, for example, [a server config that specifies the display descriptor for the Sensics dSight HMD](https://github.com/OSVR/OSVR-Core/blob/master/apps/sample-configs/osvr_server_config.dSight.json). (If no display line is specified, then a default is used.) A useful minimal sample to test your display descriptor (does not test distortion) would be the OpenGLSample in the OSVR-Core distribution (requires SDL), which places you in a cube.

## Correcting lens and display distortion
In many cases, displays have some degree of distortion that should be compensated for ahead of time through predistortion. OSVR has a number of parameterizations of distortion, suited to different types of distortion, and different tools for measuring them.  All of these modes can be specified in the server configuration files and read using the OSVR-Core libraries and several of them are implemented in the OSVR-RenderManager interface.

A theoretical description of distortion correction and its relationship to projection and viewing can be found in the [distortion document](../Configuring/distortion.md) and a program to construct the distortion parameters based on a mapping from angles to screen coordinates can be found in the [AnglesToConfig documentation](https://github.com/OSVR/distortionizer/blob/master/angles_to_config/doc/anglesToConfig.md).

The Core modes (implemented on a non-RenderManager path of OSVR-Unity) as of 1/25/2016 include:

- K1-parameter-based distortion - Based on quadratic distortion from a center for red, green, and blue.

The RenderManager modes as of 1/25/2016 include:

- Chromatic General-polynomial-based distortion - Based on radial distortion around a defined center of projection for reg, green, and blue.  See the description in Rendermanager.h for details on how this is specified.
- Monochromatic point samples - Based on an arbitrary mapping from angles to screen-space locations, often from a lens simulation performed on the optics.  See the [distortion document](../Configuring/distortion.md) for a description.
- Chromatic point samples - Based on an arbitrary mapping from angles to screen-space locations for red, green, and blue, often from a lens simulation performed on the optics.  See the [distortion document](../Configuring/distortion.md) for a description.

The process of constructing a configuration file that uses point-sampled distortion correction is described near the end of the [AnglesToConfig documentation](https://github.com/OSVR/distortionizer/blob/master/angles_to_config/doc/anglesToConfig.md).  The basic approach is to construct an appropriate server-side configuration file (with a matching client-side file if needed) and then run the standard OSVR server with that configuration file.  Any RenderManager-based client will then perform the specified distortion correction during rendering.

A snippet from a configuration file that specifies general-polynomial-based distortion follows:

    {
      "display": {
        "hmd": {
          "distortion": {
            "type": "rgb_symmetric_polynomials",
            "distance_scale_x": 1,
            "distance_scale_y": 1,
            "polynomial_coeffs_red": [ 0, 1, 0.25 ],
            "polynomial_coeffs_green": [ 0, 1, 0.32 ],
            "polynomial_coeffs_blue": [ 0, 1, 0.40 ]
          }
        }
      }
    }

## Supporting Direct Rendering
The next step is direct-to-HMD rendering: bypassing operating system and window management overhead, removing the display from being a part of the extended desktop, and drawing directly to it using the graphics driver. OSVR's [RenderManager](https://github.com/sensics/OSVR-RenderManager) handles both non-direct advanced rendering (timewarp, advanced predistortion, etc.) available with just a regular display descriptor as above, as well as direct mode support.

Using direct rendering on a new HMD requires three things: 

1. Construct a RenderManager configuration file with a screen resolution and rotatation that matches one of the modes supported by the HMD.

2. Add your EDID vendor ID to [RenderManager](https://github.com/sensics/OSVR-RenderManager/blob/master/osvr/RenderKit/DirectModeVendors.h#L227) and create a pull request to have it included in RenderManager.

3. Contact the graphics card manufacturers to have your EDID vendor ID added to their direct mode whitelists. The display must be recognized as a direct-mode display by the vendor's driver.  This is handled differently by each vendor, and you should contact them directly to be added to their whitelists. You'll need to contact nVidia to get them to add your device to their driver whitelist. Be aware that they have stopped direct-mode support for non-HDCP-compliant HMDs on laptops and on systems with multiple GPUs (and they are warning that we should support it on any display going forward). AMD has a different approach to getting devices registered, so you should contact them as well.  Intel cards require the HMDs to add a specific registry entry to indicate that they should be used in DirectMode.

## Tracking and Status Reporting
Your HMD presumably has tracking either integrated or attached. If it's an off-the-shelf tracker, then there's probably already support in OSVR for it via VRPN: see more on the [Compatibility page](http://osvr.github.io/compatibility/). A tracker for an HMD should provide the /me/head alias at the center point between the two eyes centers.

If you have a custom tracking system, and/or if your device has an interface for control and status messages, you might be interested in writing a device plugin to provide more interaction with the OSVR system than just as a display device. Get in touch, and/or see the [Building a Plugin](http://osvr.github.io/build-with/#building-a-plugin) documentation.



## Verifying your HMD is working with OSVR

You can verify that the tracker is working by using [OSVR Tracker Viewer](https://github.com/osvr/OSVR-Tracker-Viewer). It will display a set of axes showing the position and orientation of your HMD as you move it about.

You can check that the display descriptor and distortion correction is working well by running one of the demo applications that ship with [OSVR-RenderManager](https://github.com/sensics/OSVR-RenderManager#example-programs).

Finally, we also suggest using the [SteamVR-OSVR driver](https://github.com/osvr/SteamVR-OSVR) to test your HMD with SteamVR games.

## Publishing your HMD plugin

To make your HMD plugin available to the widest audience of OSVR users, we recommend the following:

 * Create simple installer to deploy the plugin. 
 * Add the installer to the OSVR plugin repository.
 * If applicable, open-source your plugin code so that the community can assist in porting it to other platforms and improve it.

