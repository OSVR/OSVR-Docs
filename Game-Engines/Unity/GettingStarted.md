# OSVR for Unity Developers

> Last Updated January 14, 2016

## Why use OSVR?

Open-source Virtual Reality (OSVR) is an open-source software platform for VR/AR applications. It provides abstraction layers for VR devices and peripherals so that a game developer does not need to hardcode support for particular hardware. 
OSVR provides interfaces – pipes of data – as opposed to an API tied to a specific piece of hardware. If you want hand position, for example, OSVR will give you hand position regardless of whether the data comes from a Microsoft Kinect, Razer Hydra, or Leap Motion. 
Game developers can focus on what they want to do with the data, as opposed to how to obtain it.

If you’re not convinced yet, or would like to read a more complete introduction to OSVR, please visit: http://osvr.github.io/whitepapers/introduction_to_osvr/

The rest of this document assumes you generally understand the benefits of OSVR and would like to know how to start using it. Before we get into the Unity plugin, let’s make sure OSVR is up and running.

## OSVR-Core and OSVR Server

When using OSVR, you need to run OSVR server. For more information about OSVR server, visit: https://github.com/OSVR/OSVR-Docs/tree/master/Getting-Started


## Getting Started with OSVR-Unity
Note that the OSVR-Unity plugin available on the Unity Asset Store may be slightly behind the Github version, but that currently, the Unity Asset Store version contains additional DLLs for RenderManager that are not contained in the OSVR-Unity.unitypackage distributed on Github (these DLLs are available in the OSVR-Unity examples project, however).

Let’s examine the OSVR-Unity plugin. 
* Download the latest Unity snapshot build from http://osvr.github.io/build-with/.
* Create a new Unity 5.3 project.
* Import OSVR-Unity.unitypackage into the project.
* Open VRFirstPerson.unity scene.

This scene demonstrates a first-person controller VR setup. Let’s examine some of the objects in the scene hierarchy:

### ClientKit
The ClientKit object communicates with OSVR Server and must be in every scene. It requires an app ID, which can be any string identifier.

![OSVR-Unity ClientKit](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_clientkit.png?raw=true)

### VRDisplayTracked
This prefab contains a DisplayController component, which constructs our tracked camera rig at runtime.

![OSVR-Unity VRDisplayTracked](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_vrdisplaytracked.png?raw=true)

Press Play. The stereo camera rig is created at runtime and updates its orientation as the HDK moves around. With my HDK connected as an extended display, I can click-and-drag the Game View to the HDK display and select Maximize on Play to view the game in VR.


![OSVR-Unity SBS](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_sbs.png?raw=true)

### Recenter

![OSVR-Unity Recenter](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_recenter.png?raw=true)

The **SetRoomRotationUsingHead** script “recenters” the room when a key is pressed (“R” by default in this sample), so that your head is facing the forward direction. There is another key (“U” by default in this sample) for undoing that rotation.

### VRFirstPersonController

![OSVR-Unity FPS Controller](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_fpc.png?raw=true)

This prefab provides very similar controls to the Unity first-person controller scripts. Mouse-look can be controlled with the “M” key, by default.

That covers the basics! There are examples of tracked controllers, eyetrackers, and locomotion in other example scenes (see TrackerView.unity). If you want to enable features like positional tracking and Direct Mode rendering, that all happens in the server configuration file.

## OSVR Server Configuration - Adding Positional Tracking and RenderManager 
For more information about configuring OSVR server for positional tracking and/or RenderManager support, visit: https://github.com/OSVR/OSVR-Docs/tree/master/Getting-Started

## Positional Tracking - Calibration
You’ll want to calibrate the camera before using positional tracking, and every time the camera moves. For a full guide on calibration, visit: https://github.com/OSVR/OSVR-Core/wiki/Video-Based-Tracking-Calibration

### Tracker View - Positional Tracking
Let’s run the server and check Tracker View again to see if we’re getting positional tracking.

![Tracker View Positional](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/tracker_view_pos.png?raw=true)

Yes! The gizmo is no longer fixed at the origin, and moves around with the HMD. 

## RenderManager
For a more information about RenderManager, including an overview of configuration options, visit: https://github.com/OSVR/OSVR-Unity-Rendering/blob/master/README.md

### Enabling RenderManager in Unity
The following additional DLLs are required in Assets/Plugins/x86_64:
* glew32.dll
* SDL2.dll
* osvrUnityRenderingPlugin.dll -- built from https://github.com/OSVR/OSVR-Unity-Rendering
* osvrRenderManager.dll

The easiest way to obtain working binaries is to copy them over from one of the example projects Assets/Plugins/x86_64 folders:
https://github.com/OSVR/Unity-VR-Samples/tree/master/Assets/Plugins/x86_64
https://github.com/OSVR/OSVR-Unity-Palace-Demo/tree/master/Assets/Plugins/x86_64

The OSVR-Unity Asset Store package also contains these DLLs, but they may be more out-of-date than on Github.

Going back to our original Unity project, if we update our Plugins folder with the DLLs, make sure the server is running, and press play, we are now in DirectMode with positional tracking! 

### Direct Mode Preview
You can see the game view mirrors the HDK display because the “Show Direct Mode Preview” option is checked on the VRDisplaytTracked prefab. It is a rather suboptimal implementation of mirror mode, and will soon be replaced by an equivalent RenderManager feature.

![OSVR-Unity Direct Mode Preview](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_directmode_preview.png?raw=true)

### Optimizing RenderManager in Unity
There are a number of configuration options available when using RenderManager. We’ve found that settings may vary per application, so developers should experiment until an acceptable balance of visual quality and fast rendering is achieved. Generally, the configuration below is a good starting point. See the RenderManager document for more information: https://github.com/OSVR/OSVR-Unity-Rendering/blob/master/README.md

![OSVR-Unity RenderManager Config](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_rendermanager_config.png?raw=true)

### Troubleshooting RenderManager in Unity
For troubleshooting RenderManager, visit: https://github.com/OSVR/OSVR-Docs/blob/master/Troubleshooting/RenderManager.md#troubleshooting-rendermanager-in-unity.

## Quality Settings
Quality settings will differ per machine/graphics card capabilities. Make sure V Sync Count is set to Don’t Sync. Otherwise, we generally recommend that using as much antialiasing as can be afforded without negatively affecting performance. Disable shadows unless your game mechanics demand it. Use Lightmapping instead of real-time lights. Use occlusion culling.

![OSVR-Unity Quality Settings](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_quality.png?raw=true)

## Player Settings
Generally, use Static and Dynamic batching to reduce draw calls. Note that the “Virtual Reality Supported” checkbox does not need to be checked, since Unity currently only supports Oculus and GearVR with this feature. Leaving it checked should not impact OSVR performance, but you will see a “[VRDevice] Initialization of device oculus failed” message that can be ignored.

![OSVR-Unity Player Settings](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_player.png?raw=true)

## Building for Android
Building an OSVR Android app requires libraries from https://github.com/OSVR/OSVR-Android-SDK.

Copy the .so files to the Assets/Plugins/libs/armeabi-v7a folder:

![OSVR-Unity Android Libs](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_androidlibs.png?raw=true)

Make sure your project is set to build for Android:

![OSVR-Unity Android Build](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_unity_androidbuild.png?raw=true)

Please visit this page for running the OSVR Android Server Launcher app: https://github.com/OSVR/OSVR-AndroidServerLauncher
