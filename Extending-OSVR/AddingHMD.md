# Adding a New HMD to OSVR

## Introduction
OSVR creates an abstration layer for many types of devices, including HMDs. This means that the same application can run on different HMDs simply by changing the configuration file that the OSVR server processes when it launches. Adding your HMD to OSVR instantly allows your customers to enjoy the OSVR applications. This note explains the key steps in adding an HMD to OSVR. These are:

- Create a display descriptor. This specifies the key attributes of the HMD such as the field of view
- Provide information about distortion. OSVR can apply distortion correction for the HMD. This step is optional.
- Support direct rendering. When used with a supported graphics card, OSVR provides direct rendering support to HMDs and this section discusses how to add this capability to your HMD through OSVR
- Often, HMDs also include peripherals such as an orientation tracker. The section on the tracker points you at the right documents towards adding the HMD tracker.

## Creating a Basic display descriptor
To start with OSVR, all you need to get a display going initially is just a "display descriptor" JSON file. You can start with a very simple one - the one for the OSVR HDK 1.0-1.2 is about as simple as they get - and modify it to suit your display.

- Format Documentation: <https://github.com/OSVR/OSVR-JSON-Schemas/blob/master/doc/display_descriptor_schema_v1/Structure%20of%20JSON%20descriptor%20file%20for%20HMDs.md>
- JSON Schema: <https://github.com/OSVR/OSVR-JSON-Schemas/blob/master/display_descriptor_schema_v1.json>
- Existing descriptors for additional reference: <https://github.com/OSVR/OSVR-Core/tree/master/apps/displays> - see particularly [the descriptor for the HDK 1.0-1.2](https://github.com/OSVR/OSVR-Core/blob/master/apps/displays/OSVR_HDK_1_1.json)

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

## Adding Distortion Correction
In many cases, displays have some degree of distortion that should be compensated for ahead of time through predistortion. OSVR has a number of parameterizations of distortion, suited to different types of distortion, and different tools for measuring them.  All of these modes can be specified in the server configuration files and read using the OSVR-Core libraries and several of them are implemented in the OSVR-RenderManager interface.

A theoretical description of distortion correction and its relationship to projection and viewing can be found in the [distortion document](../Configuring/distortion.md) and a program to construct the distortion parameters based on a mapping from angles to screen coordinates can be found in the [AnglesToConfig documentation](https://github.com/OSVR/distortionizer/blob/master/angles_to_config/doc/anglesToConfig.md).

The modes as of 1/25/2016 include:

- K1 parameter distortion - Based on quadratic distortion from a center for red, green, and blue (Implemented on a non-RenderManager path of OSVR-Unity).
Other distortion params - RM apps - typically the D3D Present Mode example.

[@todo How can the author test that this is working?]


## Supporting Direct Rendering
The final step is direct-to-HMD rendering: bypassing operating system and window management overhead, removing the display from being a part of the extended desktop, and drawing directly to it using the graphics driver. OSVR's RenderManager handles both non-direct advanced rendering (timewarp, advanced predistortion, etc.) available with just a regular display descriptor as above, as well as optional direct mode support, which is generally only available on some combinations of hardware vendors and software platforms.


## Tracking and Status Reporting
Your HMD likely has tracking integrated or attached. If it's an off-the-shelf tracker, then there's probably already support in OSVR for it via VRPN: see more on the [Compatibility page](http://osvr.github.io/compatibility/). A tracker for an HMD should provide the /me/head alias at the center point between the two eyes centers.

If you have a custom tracking system, and/or if your device has an interface for control and status messages, you might be interested in writing a device plugin to provide more interaction with the OSVR system than just as a display device. Get in touch, and/or see the [Building a Plugin](http://osvr.github.io/build-with/#building-a-plugin) documentation.

Tracking can be tested with the OSVR Tracker Viewer (can be downloaded from http://osvr.github.io/using/ - by default it will attempt to show the /me/head alias.
