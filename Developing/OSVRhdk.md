## OSVR HDK
### Head tracker format
The head tracker shows as a generic HID device, and input reports are continuously supplied at a high rate (400/sec except on early hardware not capable of that rate) except during device transitions (HDMI state transitions, etc.) that may temporarily suspend tracker reporting.

Protocol for it is as follows (in byte offsets):

- 0:
  - Bits 0:3 : Report version number, currently 3
  - Version 3 only: bit 4: "1" if video is detected and "0" if not.
  - Version 3 only: bit 5: "1" if portrait mode (1080x1920 video) is detected and "0" if landscape (1080x1920).
- 1: message sequence number (8 bit)
- 2: Unit quaternion i component LSB
- 3: Unit quaternion i component MSB
- 4: Unit quaternion j component LSB
- 5: Unit quaternion j component MSB
- 6: Unit quaternion k component LSB
- 7: Unit quaternion k component MSB
- 8: Unit quaternion real component LSB
- 9: Unit quaternion real component MSB

Each quaternion is presented as signed, 16-bit fixed point, 2’s complement number with a Q point of 14

In version 1 reports:

- 10-31: reserved for future use

In version 2 and 3 reports:
- 10-11: Gyroscope X axis velocity in radians/seconds
- 12-13: Gyroscope Y axis velocity in radians/seconds
- 14-15: Gyroscope Z axis velocity in radians/seconds
- 16-31: reserved for future use or not transmitted

Each velocity is presented as signed, 16-bit fixed point, 2’s complement number with a Q point of 9. These angular velocities should be treated as "exponential coordinates" and are local/body-relative, not global or room/world-relative.

Version 2 reports:
- Add angular velocity data.

Version 3 reports:
- Repurposes high-order bits in byte 0 (see above) for video detection/status
