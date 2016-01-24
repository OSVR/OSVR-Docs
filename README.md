# OSVR Developer Documentation

# Getting Started
- [Introduction to OSVR](http://osvr.github.io/whitepapers/introduction_to_osvr/)
- List of supported devices, engines and operating systems [is here](http://osvr.github.io/compatibility/)
- List of OSVR partners [is here](http://osvr.org/partner.html)
- [OSVR presentations and speaker notes](http://osvr.github.io/presentations/)


## Installing OSVR
- [Windows](Getting-Started/Installing/windows.md)
  - [Download pre-compiled binaries and drivers](http://osvr.github.io/using/)
- [Linux](Getting-Started/Installing/linux.md); [Build instructions](https://github.com/OSVR/OSVR-Core/wiki/Linux-Build-Instructions)
- Android
- [OS X](Getting-Started/Installing/osx.md)

## Running the OSVR Server

## Configuring OSVR
- [Calibrating the video-based tracker](https://github.com/OSVR/OSVR-Core/wiki/Video-Based-Tracking-Calibration)
- Device-specific configuration
  - [OSVR HDK](Configuring/osvrhdk.md)
- [Integrating with VRPN devices](https://osvr.github.io/whitepapers/vrpn_in_osvr/)

## Utilities
- [TrackerViewer](https://github.com/OSVR/OSVR-Tracker-Viewer/blob/master/README.md)
- [OSVR Control](Utilities/OSVRControl.md)
- OSVR_print_tree
## Troubleshooting
- [Device-specific information](Troubleshooting/DeviceSpecific.md)
- [Performance optimization using Event Tracing for Windows](http://osvr.github.io/presentations/20150901-Intro-ETW-OSVR/)

# Integrating with Game Engines

- [Unity](https://github.com/OSVR/OSVR-Unity/blob/master/GettingStarted.md)
  - [Tutorial video](https://www.youtube.com/
watch?v=TtLn6XpEisw)
  - [Migrating Oculus Unity apps to OSVR](http://access.osvr.com/binary/download/misc_assets/Migrating%20Unity%20applications%20from%20Oculus%20to%20OSVR%20Mar-11-15.pdf)
- [Unreal](https://github.com/OSVR/OSVR-Unreal/blob/master/README.md)
  - [Tutorial video](https://www.youtube.com/watch?v=u4Y9pUisL1M)
- [OpenVR/SteamVR](https://gitter.im/OSVR/SteamVR-OSVR)
- [Blender](https://github.com/BlendOSVR/OSVR-Blender)
- WebVR
- CryEngine
- [Monogame: video](https://www.youtube.com/watch?v=doOOLaIuj48)
- OpenGL

# Developing with OSVR
- Setting up the development environment
  - [Windows](https://github.com/OSVR/OSVR-Core/wiki/Windows-Build-Environment)
- [Directory of projects](http://osvr.github.io/contributing/)
- [Creating an OSVR project](Developing/creating.md)
- [Useful resources and tools](Developing/resources.md)

# Extending OSVR
- [Adding a New HMD](Extending-OSVR/AddingHMD.md)
- [Adding a new Device](https://github.com/OSVR/OSVR-Core/wiki/Adding-a-New-Device)
  - [Best practices for device descriptor](https://github.com/OSVR/OSVR-Core/wiki/Device-Descriptor-Practices)
- [Writing an OSVR Plugin](http://resource.osvr.com/docs/OSVR-Core/TopicWritingDevicePlugin.html)
- [Writing an OSVR Client](http://resource.osvr.com/docs/OSVR-Core/TopicWritingClientApplication.html)
- Porting to a New Operating System

# Roadmap
- In September 2015, RoadToVR published an article by [Yuval Boger](https://twitter.com/osvrguy) of Sensics about the OSVR roadmap. It is an excellent starting point to understand the OSVR big picture and roadmap. [The article can be found here](http://www.roadtovr.com/osvr-roadmap-creating-an-ecosystem-of-interoperable-vr-hardware-and-software/).
- The [OSVR Waffle Board](Roadmap/waffle.md) contains an overview of issues currently in GitHub issue trackers for all OSVR framework projects.
- Additional development priorities suggested by the core OSVR team can be [found here](Roadmap/additional.md).

# Getting Support
## Free support
- Post an issue on the [OSVR Github projects](https://github.com/osvr) to be addressed by the OSVR community
- email "support at osvr.org" to be addressed by core developers
- [Visit the OSVR development chat rooms](https://gitter.im/orgs/OSVR/rooms)

##Paid support
Some companies such as [Sensics](http://sensics.com/contact-us/) also provide premium support services for OSVR such as phone support, system engineering, authoring drivers, phone support or integration help. often for an hourly fee. If you require support beyond the free support options, consider contacting the premium providers.

# Additional information
- [OSVR mailing lists and newsletters](http://osvr.github.io/mailing-lists/)
- [OSVR marketing Web site](http://www.osvr.org)
- VR Knowledge Nuggets
  - [Tracking](VR-Knowledge-Nuggets/tracking.md)
  - [Displays](VR-Knowledge-Nuggets/displays.md)
