# Using Multi-Resolution Shading with Unreal



Multi-resolution shading is a technique to increase performance of VR rendering. It provides high resolution at the center of the image and lower resolution at the peripheral areas. By doing so, rendering performance can increase by up to 50% with little or no noticeable quality degradation.



NVIDIA has a fork of the UnrealEngine targeting their VRWorks SDK. Among other features, this fork includes support for multi-resolution shading on NVIDIA hardware. In this implementation, each eye is split up into viewports. The center viewport is rendered at a higher resolution than the outer viewports. 

The OSVR plugin works out of the box on the NVIDIA fork of the Unreal engine. 

- Visit this page to download the fork for the 4.12 version of the engine: https://developer.nvidia.com/nvidia-vrworks-and-ue4

- You will need access to the Unreal source code, which you obtain through Epic Games in the same way you would for the mainline branch of the engine. Once you've downloaded the fork, you should update the source code for the OSVR plugin, as the version included in the NVIDIA fork is out of date. The latest source for the OSVR-Unreal plugin is located here: https://github.com/osvr/osvr-unreal

- The source code for the OSVR plugin in the engine is located in several places, and differs from the layout in the OSVR-Unreal project:

	- UnrealEngine/Engine/Binaries/ThirdParty/OSVRClientKit (update with latest DLLs from the OSVR SDK)

	- UnrealEngine/Engine/Source/ThirdParty/OSVRClientKit (update with latest source from /OSVRUnreal/Plugins/OSVR/Source/OSVRClientKit)

	- UnrealEngine/Engine/Plugins/Experimental/OSVR (update with latest source from /OSVRUnreal/Plugins/OSVR, except leave out /OSVRUnreal/Plugins/OSVR/Source/OSVRClientKit)



- Once you've updated the source code for the OSVR plugin, build the Unreal engine as usual. To enable MultiRes rendering in your project, find the MultiRes setting in the Rendering page of of your project settings. You may also want to turn on instanced rendering. You will also need to set the vr.MultiResRendering variable to 1, as it defaults to 0. You can set this variable from a blueprint or from a C++ module in your game.