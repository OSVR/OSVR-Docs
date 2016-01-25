## Extended desktop is visible, unable to enter direct mode
- (I know this sounds boilerplate, but it works reliably on older versions of the OSVR HDK.) Unplug the USB, HDMI, and wall-power connectors from the HDK.  Plug in the wall power.  Plug in the USB connector.  Wait 10 seconds for the devices to be recognized.  Plug in the HDMI connector.
- Verify that your HMD is supported by RenderManager. At present (Jan 2016), the OSVR HDK, Vuzix iWear 720, and Raw VRPN Oculus Rift DK2 are supported.
- Verify that you have a supported graphics card. At present (Jan 2016), NVIDIA graphics cards with Gameworks VR are supported. HMD should be connected into a video port that directly goes into the NVIDIA device. If your video port goes into an Intel chip (e.g Intel Optimus configuration), direct mode will not work on that port.
- Run 'EnableOSVRDirectMode' from the shortcuts created by the Render Manager.
- If you are using an older OSVR HDK, try whitelisting your HMD using the whitelist shortcuts in the Render Manager installation. This  requires computer restart to take effect.
