# OSVR HDK Firmware Stand-alone Downloads

For non-Windows users, who therefore can't use the [OSVR-Control](OSVRControl.md) integrated tool to get the latest firmware.

## HDK 2

Do not install HDK 1.x firmware on the HDK 2!
Version numbers may look similar starting around 1.95, due to a common codebase, but substantial differences in the video signal path make them completely incompatible.

- [hdk2svr-2.00.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/hdk2svr-2.00.hex)
	- SHA-256 hash: `36c7eb20dec518c400b8cd8eda0189c9c1622be05c4fb87ced2359f9667593a0`
	- 2.00 for dual-screen, 90Hz HDK2.
	- **Overview:** This build incorporates a design change that should allow proper functioning on all HDK2 devices/systems: both the devices that worked well with 1.99, as well as the ones that didn't work well on 1.99 and instead needed to use 1.98 or earlier. If your display still doesn't work with this firmware, please contact support with the same information requested for version 1.99 debugging. Minor build warning fixes for all devices are also included that should not affect functioning relative to 1.99.
	- Changes for all devices: Fixes for some build warnings.
	- Changes for HDK 2:
		- One design change included in 1.99 was moving from polling to interrupts on HDK2-based hardware. While this worked well on the dev systems tested before release, some devices in the field (and eventually, one in our labs) didn't handle it well, seemingly not propagating the interrupt. Manually triggering a poll would often temporarily resolve the issue for setups that didn't work well with 1.99.
		- So, while this new release keeps all the new display code from 1.99, it switches to infrequently polling the video status, instead of using interrupts from the display bridge, for reliability across the full range of devices/systems in the field. It's still doing the polling in a more light-weight way than 1.98 and earlier versions, so performance shouldn't be impacted negatively compared to 1.99.
		- (It's still not clear why exactly some propagate the interrupt and some don't, but since the overall firmware family was already set up to be able to support either method, there should be no harm in switching all HDK2 devices over to polling.)

- [1-99-HDK2SVR.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-99-HDK2SVR.hex)
	- SHA-256 hash: `5d8dad93b5c7f2d24e0079870755b7b735a9a59959768a664ef5932c77f00cba`
	- 1.99 for dual-screen, 90Hz HDK2.
	- **Overview:** This release primarily affects the HDK2 and derived devices, improving their reliability and compatibility with nearly all systems and environments, as verified with broad pre-release testing. A fix for loading the stored IMU mode setting (whether or not the mag is used to mitigate gyro drift) that applies to all HDK (1.x and 2) based devices is also included.
	- Changes:
		- Re-factoring/rewrite of display-related code for HDK2-based HMDs.
		- Fixed handling of HDMI signal acquisition/loss handling for HDK2 (plug/unplug or direct mode app launch/exit).
		- Interrupt-based control of the Toshiba chip in the HDK2, improving performance by removing polling overhead.
		- Improved display timings and EDID data (in HDK2SVR variant)
		- In HDK2/HDK2SVR, inclusion of a truncated version of the text serial number in the EDID ("CT" prefix removed to fit the serial into the 13 characters/bytes available in EDID).
		- Troubleshooting and debugging command support added for HDK2.
	- Fixes these HDK2 issues, among others:
		- HDK2 not being recognized as HDCP capable by NVIDIA drivers in direct mode (and thus incompatible with direct mode on mobile - laptop or backpack/small form factor PC - due to NVIDIA driver policy)
		- Direct mode present calls failing immediately or after one or more frames are displayed.
		- A number of undifferentiated "black screen" issues not due to hardware defects.
		- Bright bar appearing on display after HDMI unplug.
	- Fixes for all HDK-family devices (all devices with BNO070 IMU):
		- Fixed loading of setting from EEPROM that determined IMU configuration: game rotation vector (aka "GRV" - set with `#sg1` - default - mag not used) vs. rotation vector (aka "RV" - set with `#sg0` - mag used to counter drift and establish fixed north direction).
	- Additional improvements, all devices:
		- git-based version stamp augmentation (for `#?v` and `#?f` output) to distinguish releases from dev builds and dev builds from each other. (This build should say "(RELEASE)" after the version number.)
		- HDK2 serial number reading (from EEPROM) is now cached to reduce calls to the NVM controller and improve reliability.
	- Known issues:
		- A small number of HDK 2-based setups, so far only noticed with NVIDIA GPUs, do not produce or propagate the video sync change interrupt properly, particularly on signal loss. This can result in a bright bar after direct mode app close or HDMI unplug until the HMD is power-cycled, or in rare cases, trouble running direct mode applications (applications will run but screen will remain dark).
			- This issue is under investigation. If you experience it, please contact OSVR Support, mentioning "OSVR-HDK-MCU-Firmware issue #17" and including the following information:
				- Your hardware and software setup - GPU, GPU driver version, OSVR server version and config file in use, and test applications - testing with the RenderManager Direct3D "Present" demo ("cube room") recommended
				- Your HDK2 headset serial number - takes the form `CTxxxxV001yyyyy`, found on the cable going from the HMD to the belt box, or on most units, using the `#fsr` command (let us know if that reports `NG` instead of your serial number)
				- Console log from OSVR-Control (or other serial terminal client) connected to the HDK before and during the undesired behavior.
				- Information reported from the `#hr` command (run in each unique condition you can put your device in)
				- Whether any of the following commands resolved the problem:
					- `#sn` (display on)
					- `#sf` (display off)
					- `#hp` (trigger HDMI sync status polling)
			- In all known cases, reverting to 1.98 can work around this issue.
		- Having the USB device powered and plugged in to a Windows 10 PC results in a blue-screen (`WDF_VIOLATION`) in a built-in Microsoft driver at system shutdown/restart, and may interfere with system suspend. Tracked at <https://github.com/OSVR/OSVR-Core/issues/389> (This is not a new issue, just has not been listed in the Known Issues section of release notes before.) The workaround is to unplug any one of the following cables before shutting down or restarting: belt box power, USB (from belt box to PC), HMD cable to beltbox. (Belt box power or USB to PC are probably the preferred options due to the durability of those connectors.)

- [1-98-HDK2SVR.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-98-HDK2SVR.hex)
	- SHA-1 hash: `806dfbba80e08ffbcef2b286d96d4d63a039d3ad`
	- 1.98 for dual-screen, 90Hz HDK2.
	- Changes:
		- Fixes issue with USB serial buffering that could result in a non-responsive HMD after several display transitions or direct-mode app start/exits. (This fixes the known issue listed in the 1.97 release notes)
		- General USB and USB serial reliability and performance enhancements.
		- Adds `#?h` hardware info console command, currently reporting the status of the brownout-detection fuses.

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

- [hdk_oled-HDK-1.3-1.4-2.00.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/hdk_oled-HDK-1.3-1.4-2.00.hex)
	- SHA-256 hash: `e0f9e71ceafa2e5905a4f0cd44739253bca70c67fc4cd393393cd1aad192b7aa`
	- 2.00 for low-persistence OLED (HDK 1.3/1.4)
	- **Overview:** This release primarily affects the HDK2 and derived devices. Minor build warning fixes for all devices are also included that should not affect functioning relative to 1.99.
	- Changes for all devices: Fixes for some build warnings.
- [1-99-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-99-OLED.hex)
	- SHA-256 hash: `ef05bd83a324eea3d3b86344d8f80cb70ea75394ac12061861547e04c19f0bd0`
	- 1.99 for low-persistence OLED (HDK 1.3/1.4)
	- **Overview:** This release primarily affects the HDK2 and derived devices, improving their reliability and compatibility with nearly all systems and environments, as verified with broad pre-release testing. A fix for loading the stored IMU mode setting (whether or not the mag is used to mitigate gyro drift) that applies to all HDK (1.x and 2) based devices is also included.
	- Fixes for all HDK-family devices (all devices with BNO070 IMU):
		- Fixed loading of setting from EEPROM that determined IMU configuration: game rotation vector (aka "GRV" - set with `#sg1` - default - mag not used) vs. rotation vector (aka "RV" - set with `#sg0` - mag used to counter drift and establish fixed north direction).
	- Additional improvements, all devices:
		- git-based version stamp augmentation (for `#?v` and `#?f` output) to distinguish releases from dev builds and dev builds from each other. (This build should say "(RELEASE)" after the version number.)
	- Known issues:
		- Having the USB device powered and plugged in to a Windows 10 PC results in a blue-screen (`WDF_VIOLATION`) in a built-in Microsoft driver at system shutdown/restart, and may interfere with system suspend. Tracked at <https://github.com/OSVR/OSVR-Core/issues/389> (This is not a new issue, just has not been listed in the Known Issues section of release notes before.) The workaround is to unplug any one of the following cables before shutting down or restarting: belt box power, USB (from belt box to PC), HMD cable to beltbox. (Belt box power or USB to PC are probably the preferred options due to the durability of those connectors.)

- [1-98-OLED.hex](http://resource.osvr.com/public_download/FirmwareUpgrade/hdk-hex/1-98-OLED.hex)
	- SHA-1 hash: `d0412876178b50e72c5436308c52270a3b4a9265`
	- 1.98 for low-persistence OLED (HDK 1.3/1.4)
	- Changes:
		- Fixes issue with USB serial buffering that could result in a non-responsive HMD after several display transitions or direct-mode app start/exits. (This fixes the known issue listed in the 1.97 release notes)
		- General USB and USB serial reliability and performance enhancements.
		- Adds `#?h` hardware info console command, currently reporting the status of the brownout-detection fuses.
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
