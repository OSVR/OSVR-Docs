# Updating the HDK firmware from Linux

This will describe the process to update the HDK firmware from CLI in Linux. This guide expects a certain familiarity with Linux and comfort with CLI usage.

**Note: This process has some risk associated with it. If instructions are not entered properly it may result in bricking the device.**

## Requirements

- [dfu-programmer](https://dfu-programmer.github.io/)
  - [AUR package](https://aur.archlinux.org/packages/dfu-programmer/) - For Arch Linux users
- [screen](https://www.gnu.org/software/screen/) - Included in many base installs
- sha1sum - Included in many base installs

## Checking the current firmware version

Use `screen` to connect to the HDK device. *It may not be on exactly `/dev/ttyACM0` depending on your system.*

**Note: Remember to run this with root privileges(sudo or root shell).**

```bash
screen /dev/ttyACM0
```

Once connected you should have a blank screen with the terminal cursor flashing in the upper left corner.

Enter the following exactly and press enter to execute (the '#' symbol is NOT a prompt):

```
#?v
```

It will show you your current firmware version, as well as the onboard tracker version (non-upgradeable).

To exit the screen session:

1. Press `ctrl+a`
2. Then `shift+k`
3. It will prompt for y/n to quit, select 'y'

## Download and Verify the New Firmware

See [the firmware page](HDKFirmwareVersions.md) for the latest version and checksum of firmware. Note that different firmwares apply for different versions of the HDK, so choose carefully.

**Note: Do not use a file if the checksum does not match.**

Verify the checksum of the file (replace firmware.hex with the name of the file you downloaded):
```bash
sha1sum firmware.hex
3d7f8ac7412fb9f8c11ae082c4c27e550dad8012 firmware.hex
```

## Updating Firmware

**Note: All commands in this section require root privileges.**

I recommend you watch `dmesg` or use `journalctl -f` in a separate terminal to see activity of the device attaching and changing state.

### Put the HDK MCU into bootloader mode

Use `screen` to connect to the HDK (substituting the correct device path as needed)

```bash
screen /dev/ttyACM0
```

Enter the following exactly and press enter to execute:

```
#?B1948
```

**Note: The screen session should exit, this is expected as the device is rebooting into bootloader mode.**

### Uploading the new firmware

With the HDK MCU in bootloader mode (After the usb activity in the logs stops) the new firmware can be uploaded.

Use the `dfu-programmer` utility to upload the new firmware (replace 'firmware.hex' with the name of the file that was downloaded earlier):

```bash
dfu-programmer atxmega256a3bu flash --force --suppress-bootloader-mem firmware.hex
```

Reboot the HDK MCU to normal operation using the new firmware:

```bash
dfu-programmer atxmega256a3bu launch
```
**Note: The device should again disconnect and reconnect.  The device is ready to use once the logs stop.**

