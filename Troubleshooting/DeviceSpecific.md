# Device-specific information

## OSVR HDK
### Head tracker format
The head tracker shows as a generic HID device

Protocol for it is as follows (in byte offsets):

- 0:
  - Bits 0:3 : Version number, currently 3
  - Version 3 only: bit 4: "1" if video is detected and "0" if not.
  - Version 3 only: Bit 5: "1" if portrait mode (1080x1920 video) is detected and "0" if landscape (1080x1920).
- 1: message sequence number (8 bit)
- 2: Unit quaternion i component LSB
- 3: Unit quaternion i component MSB
- 4: Unit quaternion j component LSB
- 5: Unit quaternion j component MSB
- 6: Unit quaternion k component LSB
- 7: Unit quaternion k component MSB
- 8:Unit quaternion real component LSB
- 9: Unit quaternion real component MSB

Each quaternion is presented as signed, 16-bit fixed point, 2’s complement number with a Q point of 14

In version 1 reports:

- 10-31: reserved for future use

In version 2 and 3 reports:
- 10-11: Gyroscope X axis velocity in radians/sensics
- 12-13: Gyroscope X axis velocity in radians/sensics
- 14-15: Gyroscope X axis velocity in radians/sensics

Each velocity is presented as signed, 16-bit fixed point, 2’s complement number with a Q point of 9

In version 3 reports:
- Adds bits in byte 0 (see above) for video detection

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
