## Extended desktop is visible, unable to enter direct mode
- (I know this sounds boilerplate, but it works reliably on older versions of the OSVR HDK.) Unplug the USB, HDMI, and wall-power connectors from the HDK.  Plug in the wall power.  Plug in the USB connector.  Wait 10 seconds for the devices to be recognized.  Plug in the HDMI connector.
- Verify that your HMD is supported by RenderManager. At present (Jan 2016), the OSVR HDK, Vuzix iWear 720, and Raw VRPN Oculus Rift DK2 are supported.
- Verify that you have a supported graphics card. At present (Jan 2016), NVIDIA graphics cards with Gameworks VR are supported. HMD should be connected into a video port that directly goes into the NVIDIA device. If your video port goes into an Intel chip (e.g Intel Optimus configuration), direct mode will not work on that port.
- Run 'EnableOSVRDirectMode' from the shortcuts created by the Render Manager.
- If you are using an older OSVR HDK, try whitelisting your HMD using the whitelist shortcuts in the Render Manager installation. This  requires computer restart to take effect.

## Troubleshooting RenderManager in Unity
If you see a “failed to load RenderManager” message print to the Unity debug console or output log, try running the RenderManager example programs from a command line to see the output.

If you get an error that says something like “Could not open display (are you whitelisted?)”, try running the **CombinedWhitelist.reg** file, then restart the computer.

If you see this error, then the server isn’t configured for RenderManager.

![No RenderManager Config](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_norendermanager.png?raw=true)

If the example programs work in direct mode, but Unity still fails to load RenderManager, make sure all DLLs are up to date. It is best to close Unity, replace the DLLs in Windows Explorer, and re-open Unity so that the new libraries are loaded. For more information, visit https://github.com/OSVR/OSVR-Unity/blob/master/GettingStarted.md#enabling-rendermanager-in-unity.
