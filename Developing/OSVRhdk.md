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
- 16-31: reserved for future use

Each velocity is presented as signed, 16-bit fixed point, 2’s complement number with a Q point of 9

In version 3 reports:
- Adds bits in byte 0 (see above) for video detection
