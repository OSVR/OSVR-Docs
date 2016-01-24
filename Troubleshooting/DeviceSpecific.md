# Device-specific information


## Razer Hydra

- The Razer Hydra driver package from Razer is NOT required, and in fact can sometimes conflict.
- If you have difficulties and have installed the Razer driver for the Hydra, try closing the tray application or uninstalling it—it has been known to switch the Hydra from motion controller mode back into game controller mode while in use or otherwise interfere with the OSVR server's Hydra support.
- The OSVR server should automatically detect if the Hydra is connected when it starts, and print some messages if it is.
- To calibrate the Hydra, you need to put both controllers on the base, on the appropriate sides (look at the triggers to see which is left and which is right). This will ensure hemisphere tracking is correct and will re-set the rotation, as shown below (only one controller shown for clarity). Note that this is the non-Unity coordinate system - Unity should be equivalent but with the Z (blue) pointing the opposite direction.

## YEI 3-Space Sensor

- [Driver here](http://opengoggles.org/preview/3-Space_Driver_Install.zip)
- YEI does not have a signed driver, so follow these instructions for install on Windows 8 or 8.1: http://forum.yeitechnology.com/viewtopic.php?f=3&t=24
- Note the COM port assigned to the USB-Serial device created by the driver, you will need to configure this in osvr_server_config.json.
- High COM ports (above around 8 or 10) may require specifying the port as \\.\COM13 for the time being. (GitHub issue reference: https://github.com/sensics/OSVR-Core/issues/7)
- Useful link about hidden com ports, if your numbers are getting very high: [How to Find Hidden COM Ports](https://learn.adafruit.com/how-to-find-hidden-com-ports/overview)
