## Compatible GPU Drivers
- NVIDIA:
  - **Warning**: 367-series and newer drivers have an **added limitation from NVIDIA**: they disable "multi-GPU" (when one or more is not an NVIDIA GPU) capability for direct mode on a number of HMDs, including the HDK.
    - This affects you if, for instance,
      - You use a laptop with hybrid graphics, even with an NVIDIA GPU with a direct external graphics connector as well as integrated Intel graphics that previously supported NVIDIA direct mode
      - You use a desktop Intel chip with the GPU enabled/in use in addition to a discrete NVIDIA card
    - To work around this, you'll either need to:
      - Disable the other GPU (if possible - e.g., on desktop)
      - Stay with a pre-367 driver.
    - For additional information on this, see, for example, the [Release notes for desktop drivers version 368.22](http://us.download.nvidia.com/Windows/368.22/368.22-win10-win8-win7-winvista-desktop-release-notes-updated.pdf)
  - **365.xx** should work on all systems that 362 worked on - requires that applications be built with a post-March 2016 Render Manager, but provides some additional functionality over 362. 365.10 and 365.19 (GeForce) tested.
  - Recommended to **skip 364.xx** releases - the earliest such release ever known to work (and capable of working - earlier versions were known to have bugs making them incompatible with direct mode) was 364.72, and it only worked on some systems.
  - **362.xx** works well as a fallback even with applications using very old (pre-March 2016) Render Manager versions. (362.00 for standard GeForce drivers - pre-364 for other hardware with different driver releases.)
- AMD : 16.2 or earlier as of 18 March 2016.

## RenderManagerAMDD3D11::OpenDisplay(): Could not create Liquid VR device
If you have previously installed an AMD card and then removed it, deleting the drivers, it sometimes leaves DLLs on the system.  RenderManager uses the presence of the DLL named amdlvr64.dll, which on some systems is located in C:\Windows\System32, to determine whether AMD's Liquid VR is available on the system.  If that DLL is present, it attempts to use it to open DirectMode, resulting in the error message above.  Some users have reported successful resolution of this issue by removing that DLL from their system when they do not have an AMD card installed.
 
## General troubles with NVIDIA devices: clean install.
A first line troubleshooting option is a clean-install of the NVIDIA drivers. Install the recommended NVIDIA drivers again, but choose "Custom" install, then check the box for a "Clean" install. This will remove any customized settings or profiles you may have created in the NVIDIA control panel (so if you did make any, you may want to record them or back them up), but it can also remove corrupted internal settings in the NVIDIA driver itself. When you re-install the drivers, try doing so without the HDK connected.

Once you've done a clean reinstall, it will likely automatically put the HDK into direct mode when you plug it in, which means that Windows will likely not see it as a monitor and you'll only see black on the screen - use a tool to disable direct mode to check extended mode functionality.

It is harmless, and in general, when possible recommended to do clean installs of NVIDIA drivers when you upgrade them as well.

## Extended desktop is visible, unable to enter direct mode
- (I know this sounds boilerplate, but it works reliably on older versions of the OSVR HDK.) Unplug the USB, HDMI, and wall-power connectors from the HDK.  Plug in the wall power.  Plug in the USB connector.  Wait 10 seconds for the devices to be recognized.  Plug in the HDMI connector.
- Verify that your HMD is supported by RenderManager. At present (May 2016), the OSVR HDK, Vuzix iWear 720, HTC Vive/Vive Pre, and Oculus Rift DK2 are supported.
- Verify that you have a supported graphics card. At present (May 2016):
  - NVIDIA graphics cards with Gameworks VR/VRWorks are supported. (Generally - those that can run the latest driver series.) HMD should be connected into a video port that directly goes into the NVIDIA device. If your video port goes into an Intel chip (e.g some Intel Optimus configurations), direct mode will not work on that port. You can run the OSVR "DirectModeDebugging" tool to find out if your hardware configuration supports VRWorks - it will list near the beginning of its operation whether there are any connectors on your NVIDIA GPU.
  - AMD LiquidVR DirectToDisplay capable GPUs are supported - generally, AMD "GCN" GPU cores.
- Run 'EnableOSVRDirectMode' from the shortcuts created by the Render Manager.
- If you are using an older OSVR HDK, try whitelisting your HMD using the whitelist shortcuts in the Render Manager installation. This  requires computer restart to take effect.

## Troubleshooting RenderManager in Unity
If you see a “failed to load RenderManager” message print to the Unity debug console or output log, try running the RenderManager example programs from a command line to see the output.

If you get an error that says something like “Could not open display (are you whitelisted?)”, try running the **CombinedWhitelist.reg** file, then restart the computer.

If you see this error, then the server isn’t configured for RenderManager.

![No RenderManager Config](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_norendermanager.png?raw=true)

If the example programs work in direct mode, but Unity still fails to load RenderManager, make sure all DLLs are up to date. It is best to close Unity, replace the DLLs in Windows Explorer, and re-open Unity so that the new libraries are loaded. For more information, visit https://github.com/OSVR/OSVR-Unity/blob/master/GettingStarted.md#enabling-rendermanager-in-unity.

## Error Messages and Warnings
In the case of all error and warning messages, the earlier they appear, the more important they likely are. If you're seeking help through a support channel, make sure to include as full of a log as you can, to ensure you don't accidentally exclude the message that contains the important information.

### Messages about `WSAStartup`, `vrpn_Endpoint`, or `vrpn_Connection`
These are **not meaningful on their own** - if you see one (and usually, if you see one, you'll see several in a row right at the end of a log) it's a side-effect of an earlier error message causing an unclean shutdown. Scroll up in the log/console to see what actually went wrong.

### `OSVR RenderManager Warning: Got error from NVIDIA API: EnableDirectMode returned -187 (hex: ffffff45) [NVAPI_INVALID_DISPLAY_ID]`
This message means that the NVIDIA driver doesn't recognize the display device you're trying to use in Direct Mode as being a VR display. If you're using a standard OSVR-supported device (currently including the OSVR HDK 1.3 and Vuzix iWear), try upgrading your NVIDIA drivers to make sure they have the latest list included. As of February 2016, **version 361.75** is verified to work correctly with both of those devices with no additional "whitelists" or other work. You may need to choose Custom Install, and Clean Install to clear out all driver settings and custom profiles if you have trouble.

If you have an older HDK or other HMD that you think should be supported by Direct Mode but you're still seeing this message on the latest drivers, [contact support for a whitelist file](http://support.osvr.org) for your device.

### `RenderManagerNVidiaD3D11::OpenDisplay: Cannot create high-priority rendering when D3D context and device are passed in (ignoring)`

If this message appears on its own and the app works fine otherwise, it can be ignored. It just indicates to a developer that the D3D device and context were passed in rather than created within RenderManager. However, if this shows up in your application, you might consider trying to eliminate the pattern that causes it, for a number of reasons, including but not limited to multi-GPU support: see the information below called out by the bolded text "passing in your own D3D device and context" for details.

### `OSVR RenderManager Warning: Got error from NVIDIA API: NVAPI call returned -103 (hex: ffffff99) [NVAPI_INVALID_COMBINATION]`
This message is usually caused by having a multi-GPU system (typically with different vendors).

- If you see this in someone else's app, they might be using an old version of RenderManager that didn't have multiple/heterogeneous GPU support (released January 2016), or they might be doing the code pattern below that makes their app less compatible.
- If you see this when testing your own code, it's probably because you're **passing in your own D3D device and context** to the call to create RenderManager: if it's not entirely necessary to do so, leave creation of these interfaces to RenderManager, since for direct mode to work in such a system, the device and context must be created on the adapter being used for direct mode, which is often not the default adapter. If you do it yourself, instead of leaving it to RenderManager, you'll have a less compatible application.
