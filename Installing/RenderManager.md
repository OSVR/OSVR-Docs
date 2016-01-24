# Render Manager
## What is Render Manager ?
Render Manager is a software component that is part of OSVR. It provides key render capabilities in support of VR applications:
- Direct mode. Allows not having the HMD show as an extended-mode display in Windows.
- Time warping. Makes "just in time" adjustments to the rendered image based on latest tracking data. This reduces perceived latency between motion and image changes.
- Distortion correction. Corrects for geometrical distortion and chromatic aberrations so that displayed image appears correct.

The RenderManager installation includes both the binary code required to achieve this as well as many example programs illustrating the use of RenderManager in both DirectX and OpenGL environments.

## Installation
Download and install Render Manager from
[here](http://osvr.github.io/using/)

**Important**: Using DirectMode requires nVidia driver version 361.43 or higher.

- If you have multiple GPUs not in SLI mode, you need to select "Activate all displays" instead of "SLI Disabled" in the nVidia Control Panel under "Configure SLI, Surround, PhysX" to make DirectMode work.
- If you get an error when running the RenderManager applications, you may need to install the Visual Studio redistributable runtime package.  There is a shortcut to do this installation.  This only has to be done once per computer.

## Operation
1.  Run one of the servers using the installed shortcuts:
  - osvr_server_directmode: Displays on an OSVR HDK 1.3 in DirectMode
  - osvr_server_nondirectmode: Displays on an OSVR HDK 1.3 in portrait mode at 1920x0 (right screen)
  - osvr_server_window: Displays in a window that can be moved around.
2. Run one of the example programs:
  - RenderManager*: Various graphics libraries and modes of operation.
  - AdjustableRenderingDelayD3D: Shows the impact of timewarp with 500ms rendering delay.
  - RenderManagerOpenGLHeadSpaceExample: Displays a small cube in head space to debug backwards eyes.
  - SpinCubeD3D provides a smoothly-rotating cube to test update consistency.
  - SpinCubeOpenGL provides a smoothly-rotating cube with frame ID displayed to test update consistency.  Timing info expects 60Hz DirectMode display device that blocks the app for vsync.

#### Notes
- As of version 0.6.29, DirectMode only works on nVidia cards with a driver that has been modified to white-list the display that you are using.  This should be already in the driver version 361.43 for OSVR and Vuzix displays.  To enable support for the 1.2 versions of OSVR, there is shortcut that will modify the registry.  There is another shortcut to set it back to 1.3.  Changing these requires a reboot before they take effect.
- Since version 0.6.2, an osvr_server.exe must be running to open a display (this is where it gets information about the distortion correction and other system parameters).  The osvr_server.exe must use a configuration that defines /me/head (implicitly or explicitly) for head tracking to work.  There are shortcuts to run the server with various configuration files.  You can leave the server running and run multiple different clients one after the other.  All clients should work with all servers.
- Since version 0.6.12, the configuration information for both the display and the RenderManager pipeline are read on the server.  This file can be edited to change the behavior of the display, including turning direct_mode on and off and turning vertical sync on (set vertical_sync_block_rendering true).
- Since version 0.6.3, Time Warp works on both OpenGL and Direct3D, and in version 0.6.8 a feature was added to ask it to wait until just before vsync (reducing latency by reading new tracker reports right up until vsync).  The DirectMode examples have this capability turned on.
- Since version 0.6.3, distortion correction works on both OpenGL and Direct3D.  Since version 0.6.8, distortion correction uses an arbitrary polynomial distortion with respect to the distance from the center of projection on the screen.
- Release notes and version history is in the "README.txt" file that is part of the RenderManager installation

## Source Code Access
- RenderManager comes with source code of the example programs.
- Source code for the Render Manager itself is not yet open because it includes portions that are licensed under NDA from NVIDIA.This is correct as of Jan 24, 2016. **Sensics is working to separate Render Manager into an open component and an NDA component**. Check back often to see if this has changed
