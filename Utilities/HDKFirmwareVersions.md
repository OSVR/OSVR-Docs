# OSVR HDK Firmware Stand-alone Downloads

For non-Windows users, who therefore can't use the [OSVR-Control](OSVRControl.md) integrated tool to get the latest firmware.
## HDK 2

Do not install HDK 1.x firmware on the HDK 2!
Version numbers may look similar starting around 1.95, due to a common codebase, but substantial differences in the video signal path make them completely incompatible.

- [1-97-HDK2SVR.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-97-HDK2SVR.hex)
	- SHA-1 hash: `64829dbd6d227b0424a88750e983cd553ea56f69`
	- 1.97 for dual-screen, 90Hz HDK2.
	- Changes:
		- More reliable detection of new video signals and loss/shutdown of video signal (sleep, direct mode enter, direct mode app exit) - Many fewer "black screen" issues, less need to run "start display" button (which is equivalent to sending `#hi` command through OSVR Control)
		- Fixed: Now works with DVI sources.
		- Fixed: Now works when HDCP gets enabled or if HDMI audio is unmuted (HDMI audio untested, status unknown)
		- Fixed: Direct mode now works with AMD cards (previously, would turn off the display after less than 1 second on an RX480) - 16.8.x drivers appear to be required.
		- Fixed: Screen now turns off after signal loss without leaving previous image or a horizontal bar growing in brightness.
		- Improvement: HDMI and display control procedures now properly yield during their required delay steps, permitting the tracker to be serviced, which should keep the tracker and USB responsive during these events.
	- Known issues:
		- Known issue: Some systems may briefly (~1 sec) see a horizontal bar between last full image and full display shutoff - under investigation.
		- After a number of display on/off transitions (including app begin/end in direct mode), if you do not have OSVR-Control open and connected to the virtual serial port, the display will stop responding to the gain and loss of signal and tracker may stop reporting. You can either power-cycle the HMD, or just open OSVR-Control and click "Connect" (it's OK to do this earlier and leave this open) and the display should catch up and complete its state transitions. This may also affect HDK 1.x, to a lesser degree. Mitigations to make this less frequent are in this release, and a full fix is expected for the next one -- but we considered the advancements in this firmware too substantial to hold back on account of this issue.

- [1-96-HDK2SVR.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-96-HDK2SVR.hex)
	- SHA-1 hash: `8d09f77edf5ce438d4361cddbf11c72bf4061dbf`
	- 1.96 for dual-screen, 90Hz HDK2.
	- Changes:
		- Uses `SVR` registered PNPID for monitor hardware ID for direct mode compatibility
		- Improved EDID data.

- [1-95-HDK2.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-95-HDK2.hex)
	- SHA-1 hash: `000f3a17b5503d433e34f2642ab3b95c688398a3`
	- 1.95 for dual-screen, 90Hz HDK2 (Do not install on HDK 1.x!)
	- Earlier HDK 2 firmware version 1.01 is roughly equivalent to a 1.92 version, with some limitations.
	- Notable changes on the HDK2:
		- Re-unified codebase with OSVR HDK 1.x and other Sensics devices, resulting in restored functionality compared with 1.01.
		- USB reliability improvements.
		- Tracker performance improvements.
- [1-01-HDK2.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-01-HDK2.hex)
	- SHA-1 hash: `1a601e9a68a0e9060085b6a4c48d1a32b019f3d6`
	- Initial shipped firmware for the HDK 2, forked from roughly a common-codebase 1.92 version, with some limitations.
	- Provided for convenience in case this older version is more compatible with your particular video card.

## HDK 1.2/1.3/1.4

Do not install HDK 2 firmware on the HDK 1.x!
Version numbers may look similar starting around 1.95, due to a common codebase, but substantial differences in the video signal path make them completely incompatible.

- [1-97-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-97-OLED.hex)
	- SHA-1 hash: `38f2889289199d34346e93e04d2f38c6d26d8015`
	- 1.97 for low-persistence OLED (HDK 1.3/1.4)
	- Changes:
		- Fix for HDK 1.x regression in ~1.95: the HDK "Video Status" tool (in particular, the part of the HID input report populated with the video status, which is exposed through OSVR and that it uses to print its messages) had been mistakenly rendered nonfunctional. It is again working for HDK 1.x.
	- Known issues:
		- After a number of display on/off transitions (including app begin/end in direct mode), if you do not have OSVR-Control open and connected to the virtual serial port, the display will stop responding to the gain and loss of signal and tracker may stop reporting. You can either power-cycle the HMD, or just open OSVR-Control and click "Connect" (it's OK to do this earlier and leave this open) and the display should catch up and complete its state transitions. **This may also affect HDK 1.x, to a lesser degree.** Mitigations to make this less frequent are in this release, and a full fix is expected for the next one -- but we considered the advancements in this firmware too substantial to hold back on account of this issue.

- [1-96-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-96-OLED.hex)
	- SHA-1 hash: `861e3e660c80195dc8962d9143ba7faca0746098`
	- 1.96 for low-persistence OLED (HDK 1.3/1.4)
	- Notable changes since 1.92:
		- All known issues related to side-by-side mode have been fixed. You may need to toggle side-by-side mode once (`#fs` at the virtual USB serial port exposed by the HDK toggles between side-by-side and normal mode) to repair your saved setting. (This includes disabling HID-directed SBS mode, which was suspected to cause the problems related to SteamVR)
		- USB reliability improvements.
- [1-92-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-92-OLED.hex)
	- SHA-1 hash: `3d0284110bba7b699a5ba47792d1faf23293ab7c`
	- 1.92 for low-persistence OLED (HDK 1.3/1.4)
	- Reports build date as Mar 24 2016
	- Notable changes:
		- Adds selectable tracker mode ("Rotation Vector" using compass for absolute orientation, vs "Game Rotation Vector" default)
		- Saves and restores display mode (side-by-side) and persistence mode settings between power cycles.

- [1-91-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-91-OLED.hex)
	- SHA-1 hash: `7e93b7e514533b6ed961d1e8b44a7e4bc2458ba1`
	- 1.91 for low-persistence OLED (HDK 1.3/1.4)
	- Reports build date as Mar 1 2016
	- Notable changes:
		- Adds additional low-persistence settings, including a single-pulse mode.

- 1.89 - fixed USB disconnect issues and updated EDID data to remove a faulty 4:3 aspect ratio that confused some devices - if you upgrade past this version, you may need to run a command to update the EDID memory on the HDK.

- [1-84-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-84-OLED.hex)
	- SHA-1 hash: `3c5f298010b25e7a7b18d24d13cefb17fcc5f975`
	- 1.84 for OLED as shipped in HDK 1.2.
	- Last version without display controller commands specific to the HDK 1.3 low-persistence "silver screen" OLED - **upgrading a 1.2 OLED past this version is of unknown safety.**
