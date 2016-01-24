## Extended desktop is visible, unable to enter direct mode
- Verify that your HMD is supported by RenderManager. At present (Jan 2016), the OSVR HDK and Vuzix iWear 720 are supported.
- Verify that you have a supported graphics card. At present (Jan 2016), NVIDIA graphics cards with Gameworks VR are supported. HMD should be connected into a video port that directly goes into the NVIDIA chip. If your video port goes into an Intel chip (e.g Intel Optimus configuration), direct mode will not work on that port.
- Run 'EnableOSVRDirectMode' from the shortcuts created by the Render Manager.
- If running older OSVR HDK, try whitelisting your HMD using the whitelist shortcuts in the Render Manager installation. This might require computer restart.