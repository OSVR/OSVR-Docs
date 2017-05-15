# OSVR Developer Documentation



# Introduction to OSVR
- [Introduction to OSVR](http://osvr.github.io/whitepapers/introduction_to_osvr/)
- [OSVR feature list](featurelist.md)
  - [Notes on Android support](Introduction/Android.md)
  - [Notes on Linux support](Introduction/Linux.md)
  - [Notes on OSX support](Introduction/OSX.md)
- [List of supported devices, engines and operating systems ](http://osvr.github.io/compatibility/)
- [List of OSVR partners](http://osvr.org/partner.html)
- [OSVR presentations and speaker notes](http://osvr.github.io/presentations/)


# Using OSVR
## Getting Started
- [HDK Unboxing and Getting Started](Getting-Started/HDK/HDK-Unboxing-and-Getting-Started.md)
- [HDK Unboxing and Getting Started (SPANISH)](Getting-Started/HDK/HDK-Unboxing-and-Getting-Started-ES.md)

## Installing OSVR

- [Windows](Getting-Started/Installing/windows.md)
  - Minimum recommended version : Windows Vista SP1 or later
  - [Download pre-compiled binaries and drivers](http://osvr.github.io/using/)
  - [Install and test RenderManager](Installing/RenderManager.md)
- [Linux](Getting-Started/Installing/Linux-Build-Instructions.md)
  - Minimum recommended version : Debian Jessie or equivalent ( Ubuntu 15.04+ for example )
  - [Very generic instructions](Getting-Started/Installing/linux.md)
- [Android](https://github.com/OSVR/OSVR-Android-Build#readme)
- [OS X](Getting-Started/Installing/osx.md)
  - Minimum recommended version : El Capitan

Note : It can be used in earlier versions but might require compilation and tweaking.

## Running the OSVR Server
### Setting the OSVR_SERVER_ROOT environment variable
OSVR server auto-start functionality in ClientKit (Windows only for now) currently relies on the OSVR_SERVER_ROOT environment variable to find the server you would like to have launch. Install the [OSVR Runtime](http://osvr.github.io/using/), then set the OSVR_SERVER_ROOT environment variable to C:/Program Files/OSVR/Runtime/Server (for 64-bit) or C:/Program Files (x86)/OSVR/Runtime/Server (for 32-bit).

### Server parameters
Running the OSVR server just requires passing it a configuration file. For example:
**osvr_server osvr_server_config.json**

if no parameter is specified, a default file is used.

## Configuring OSVR
- [Using the OSVR-Config utility](Configuring/OSVR-Config.md)
- [RenderManager](Configuring/OSVR-RenderManager.md)
- [Calibrating the video-based tracker](Getting-Started/HDK/Video-Based-Tracking-Calibration.md)
- [Local and distributed client/server configurations](Configuring/LocalAndRemote.md)
- Device-specific configuration
  - [OSVR HDK](Configuring/osvrhdk.md)
  - [HTC Vive](Configuring/HTC-Vive.md)
  - [Oculus Rift](Configuring/oculus-rift.md)
- [Integrating with VRPN devices](https://osvr.github.io/whitepapers/vrpn_in_osvr/)
- [Configuring OSVR HDK2 and other OSVR-supported HMD with VIVE Puck Tracking](https://github.com/OSVR/OSVR-Docs/blob/master/Extending-OSVR/ConfiguringHDKViveTracking.md)

## Utilities
- [OSVR-Central](Utilities/OSVRCentral.md)
- Bundled with OSVR-Core binary snapshots/included in the OSVR-Core source tree:
  - [osvr_reset_yaw (e.g. "recenter")](http://resource.osvr.com/docs/OSVR-Core/OSVRResetYaw.html)
  - [osvr_print_tree](http://resource.osvr.com/docs/OSVR-Core/OSVRPrintTree.html)
  - [PathTreeExport](http://resource.osvr.com/docs/OSVR-Core/PathTreeExport.html)
  - [osvr_list_usbserial](http://resource.osvr.com/docs/OSVR-Core/OSVRListUSBSerial.html)
- [TrackerViewer](https://github.com/OSVR/OSVR-Tracker-Viewer#readme)
  - [Additional Tracker Viewer documentation](http://osvr.github.io/doc/tracker-viewer/)
- [OSVR Control](Utilities/OSVRControl.md)
- [Updating OSVR HDK Positional Tracking IR board firmware](Utilities/HDKUpgradeIRBoardFirmware.md)
- [Updating HDK Firmware from Linux](Utilities/UpdateHDKLinux.md)
- [Distortionizer](https://github.com/OSVR/distortionizer#readme) helps measure distortion parameters for HMDs
- [Latency tester](https://github.com/sensics/Latency-Test). Open-source Arduino-based latency tester
- [OSVR HDK Video Status Tool](https://github.com/sensics/OSVR-HDK-Video-Status#readme) - Find out what video signal the HDK is receiving.
  - [HDK Video Status Tool Windows Binaries](https://github.com/sensics/OSVR-HDK-Video-Status/releases) - On the Releases page - often a good place to check.

## Troubleshooting
- [FAQ](Troubleshooting/FAQ.md)
- [OSVR server](Troubleshooting/OSVRServer.md)
- [Render Manager](Troubleshooting/RenderManager.md)
- HDK troubleshooting
  - [Skewed tracker orientation](Troubleshooting/SkewedTracker.md)
  - See also the HDK video status tool above in Utilities.
- [Device-specific information](Troubleshooting/DeviceSpecific.md)
- [Performance optimization using Event Tracing for Windows](http://osvr.github.io/presentations/20150901-Intro-ETW-OSVR/)
- [Re-flash bootloader on OSVR HDK](Configuring/HDKBootloader.md)

# Developing with OSVR
- Setting up the development environment
  - [Windows](Developing/Windows-Build-Environment.md)
- [Directory of projects](http://osvr.github.io/contributing/)
- [Doxygen-generated developer documentation of OSVR-Core](http://resource.osvr.com/docs/OSVR-Core/)
- [Doxygen-generated internal documentation of OSVR-Core](http://resource.osvr.com/internal-docs/OSVR-Core-Implementation/)
- [Creating an OSVR project](Developing/creating.md)
- [OSVR interfaces](Developing/interfaces.md)
- [Useful resources and tools](Developing/resources.md)
- Device-specific information
  - [OSVR HDK HID report interface](Developing/OSVRhdk.md)

# Game Engines and OSVR

- [Unity](https://github.com/OSVR/OSVR-Unity/blob/master/GettingStarted.md)
  - [Tutorial video](https://youtu.be/xSOq3bOBPxs)
  - [Unity VR demos](https://github.com/OSVR/Unity-VR-Samples) are OSVR versions of the standard VR demos that ship with Unity 5.3
  - [Palace demo](https://github.com/OSVR/OSVR-Unity-Palace-Demo) is a Unity demo illustrating how to use the Unity OSVR plugin
  - [Unity version of Palace demo](http://github.com/OSVR/OSVR-Unity-Palace-Demo/blob/androidPalace/README.md) including APK and compilation notes
  - [Palace demo and other Unity binaries](https://github.com/OSVR/OSVR-Unity-Palace-Demo/releases)
  - [Migrating Oculus Unity apps to OSVR](http://access.osvr.com/binary/download/misc_assets/Migrating%20Unity%20applications%20from%20Oculus%20to%20OSVR%20Mar-11-15.pdf)
- [Unreal](https://github.com/OSVR/OSVR-Unreal/blob/master/README.md)
  - [Tutorial Video (Unreal Engine 4.12+)](https://www.youtube.com/watch?v=Co2IiLVS3r8&feature=youtu.be)
  - [Tutorial video (Unreal Engine 4.11 or building from source)](https://www.youtube.com/watch?v=u4Y9pUisL1M)
  - [Using multi-resolution shading with OSVR](Integrating-Game-Engines/Unreal/multires.md)
  - [Additional Documentation](https://github.com/OSVR/OSVR-Unreal/blob/master/Documentation.md)
- [OpenVR/SteamVR](https://github.com/OSVR/SteamVR-OSVR)
- [Blender](https://github.com/BlendOSVR/OSVR-Blender)
- [WebVR](Integrating-Game-Engines/WebVR/webvr.md)
- CryEngine
- [Monogame: video](https://www.youtube.com/watch?v=doOOLaIuj48)
- OpenGL

# Extending OSVR
- [Adding a New HMD](Extending-OSVR/AddingHMD.md)
- [Adding a new Device](Extending-OSVR/Adding-a-New-Device.md)
  - [Best practices for device descriptor](Extending-OSVR/Device-Descriptor-Practices.md)
- [Writing an OSVR Plugin](http://resource.osvr.com/docs/OSVR-Core/TopicWritingDevicePlugin.html)
- [Writing an OSVR Client](http://resource.osvr.com/docs/OSVR-Core/TopicWritingClientApplication.html)
- Porting to a New Operating System

# Roadmap
- [Aug 2016 article in RoadToVR on future plans for OSVR](http://www.roadtovr.com/osvr-co-founder-on-the-future-of-open-source-virtual-reality/)
- In September 2015, RoadToVR published an article by [Yuval Boger](https://twitter.com/osvrguy) of Sensics about the [OSVR roadmap](http://www.roadtovr.com/osvr-roadmap-creating-an-ecosystem-of-interoperable-vr-hardware-and-software/). It is an excellent starting point to understand the OSVR big picture and roadmap. 
- The [OSVR Waffle Board](Roadmap/waffle.md) contains an overview of issues currently in GitHub issue trackers for all OSVR framework projects.
- Additional development priorities suggested by the core OSVR team can be [found here](Roadmap/additional.md).

# Hacking the Hardware
- [iFixit guides on various HDK 1.3/1.4 upgrades and repairs](https://www.ifixit.com/Device/Razer_OSVR_HDK_1.4)
- [Replacement parts and other OSVR-related products](https://osvrstore.com/collections/frontpage)

# Getting Support

## Search
[Custom Google Search for the OSVR project](https://cse.google.com/cse/publicurl?cx=016285390483464504735:ifzwvrb3lp4)

## Free support
- Post an issue on the [OSVR Github projects](https://github.com/osvr) to be addressed by the OSVR community
- Open a support ticket on the [support portal](http://support.osvr.com/) (or email "support at osvr.org") to be addressed by core developers
- [Visit the OSVR development chat rooms](https://gitter.im/orgs/OSVR/rooms)

## Paid support
Some companies such as [Sensics](http://sensics.com/contact-us/) provide premium support services for OSVR including phone support, system engineering, authoring drivers or integration help. If you require support beyond the free support options, consider contacting the premium providers.

# Participate
A list of OSVR developer chat rooms is [here](https://gitter.im/orgs/OSVR/rooms)

Some existing rooms:
- [General subjects](https://gitter.im/OSVR/OSVR-General)
- [Unity](https://gitter.im/OSVR/OSVR-Unity)
- [Unreal](https://gitter.im/OSVR/OSVR-Unreal)
- [SteamVR](https://gitter.im/OSVR/SteamVR-OSVR)
- [OSVR Core](https://gitter.im/OSVR/OSVR-Core)

[Combined OSVR issue tracker](https://waffle.io/osvr/osvr-core)

[Map of OSVR projects](http://osvr.github.io/contributing/)

# Useful Links
Revive allows non Oculus headsets be used for games which only support Oculus headsets when run through steamvr runtime https://github.com/LibreVR/Revive
FakeVive allows non HTC headsets be used for games which only support HTC headsets when run through steamvr runtime. https://github.com/Shockfire/FakeVive
OpenVR-AdvancedSettings gives users access to many useful openvr and openvr apps settings. Users that do not have Vive controllers will need to access these settings from desktop or rely on using the hydra drivers and six sense SDK from steam tools to emulate Vive controllers to access the settings in an overlay running as a steamvr dashboard app https://github.com/matzman666/OpenVR-AdvancedSettings
For Vive Controller Emulation:
Sixense SDK for Razer Hydra https://steamdb.info/app/42300/
SteamVR Driver for Razer Hydra http://store.steampowered.com/app/491380/SteamVR_Driver_for_Razer_Hydra/
FreePIE https://github.com/AndersMalmgren/FreePIE
Opentrack https://github.com/opentrack/opentrack
FreePIE-VR-Controls https://github.com/fmaurer/FreePIE-VR-Controls

# Additional Information
- [OSVR mailing lists and newsletters](http://osvr.github.io/mailing-lists/)
- [OSVR marketing Web site](http://www.osvr.org)
- VR Knowledge Nuggets
  - [Tracking](VR-Knowledge-Nuggets/tracking.md)
  - [Displays](VR-Knowledge-Nuggets/displays.md)
- [The Sensics insights page](http://sensics.com/insight) often covers OSVR-related topics and includes tutorials such as:
  - [Key parameters in optical design](http://sensics.com/key-parameters-optical-designs/)
  - [Common eye tracker configurations](http://sensics.com/the-three-and-a-half-configurations-of-eye-trackers/)
  - [Understanding pixel density and eye-limiting resolutions](http://sensics.com/understanding-pixel-density-and-eye-limiting-resolution/)
  - [Predictive tracking](http://sensics.com/understanding-predictive-tracking/)
  - [Foveated rendering](http://sensics.com/converting-diagonal-field-of-view-and-aspect-ratio-to-horizontal-and-vertical-field-of-view/)
  - [Asynchrnous timewarp](http://sensics.com/time-warp-explained/)
  - [Geometric distortion](http://vrguy.blogspot.com/2013/07/what-is-geometric-distortion-and-why.html)
  - [Converting diagonal FOV and aspect ratio to horizontal and vertical FOV](http://vrguy.blogspot.com/2013/04/converting-diagonal-field-of-view-and.html)
  - [TV screen size vs. goggle field of view](http://vrguy.blogspot.com/2013/04/tv-screen-size-vs-goggle-field-of-view.html)
  - [How things work: the dual-elements optics of the OSVR HDK](http://vrguy.blogspot.com/2015/01/how-things-work-dual-element-optics-of.html)
  - [Binocular overlap](http://vrguy.blogspot.com/2013/05/what-is-binocular-overlap-and-why.html)
  - [What you should know about head trackers](http://vrguy.blogspot.com/2013/05/what-you-should-know-about-head-trackers.html)
  - [An overview of positional tracking technologies for VR](http://vrguy.blogspot.com/2014/05/an-overview-of-positional-tracking.html)

# Sample third-party projects that integrate OSVR into them
- [OSVR plugin for FreePie](https://github.com/thomasgauthier/FreePIE-OSVR)
