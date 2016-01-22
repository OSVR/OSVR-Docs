# OSVR Development Priorities

Please feel free to suggest additional ideas and priorities. 

## Interfaces

OSVR is built on **interfaces**. An interface is a pipe of data. A device exposes one or more interfaces, which fit into predefined interface classes. For instance, a Razer Hydra controller exposes several interfaces: a tracker interface, with two full-pose (position and orientation) sensors/channels, a button interface, an analog interface with channels for the analog triggers and the joystick axes.

A **device** - a physical entity - implements one or more interfaces classes to present those interfaces.

Our primary focus is in integrating additional devices, including:

- Intel Realsense
- Nod Ring
- MM-One and D-Box motion platforms
- Additional HMDs


A Github repository (https://github.com/OSVR/OSVR-Specs-and-Proposals) provides work area for interface definition as well as factoring of specific devices into the interfaces. 

## Analysis Plugins
A plugin is a software module that can be dynamically identified, loaded, and connected to OSVR. Plugins contain modules that implement interface classes. Though there may be more than one module in a plugin, we often refer to the individual device support, etc. in a plugin as simply a plugin. There are two types of these modules:

- **Device drivers** that implement interfaces for one or more physical devices
- **Analysis modules**, sometimes called **analysis plugins**, that implement interfaces, such as gesture engines, that do not connect directly to devices but instead receive and process data.  
For instance, a data smoothing plugin receives data from a device interface and converts it into smoothed data. A face detection plugin receives images from a camera interface and can generate an event when a face has been identified.


We would like to see work on several analysis plugins:

- An augmented reality plugin
- Gesture engines

## Game Engines

We would love to see completed integrations with:

- CryEngine 
- WebVR (currently in process)
- Bohemia VBS3
- Persagis
- WorldViz Vizrd (Python wrapper already available)

## Platforms

OSVR distributions are desired for:
- iOS
- Tizen
- PlayStation
- Windows Mobile



