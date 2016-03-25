# OSVR HDK Firmware Stand-alone Downloads

For non-Windows users, who therefore can't use the [OSVR-Control](OSVRControl.md) integrated tool to get the latest firmware.

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
