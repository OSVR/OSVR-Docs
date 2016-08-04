# Upgrading the firmware on the HDK IR Board

The OSVR HDK 1.2, 1.3, 1.4 and HDK 2 all have a similar electronics setup for their positional tracking infrared LEDs. There is a small circuit board inside the HMD (on the left as you look at the HMD from the front - below the external USB port) dedicated to driving these LEDs, with a separate STM8 microcontroller (also known as an MCU) that controls synchronizing the LEDs with the camera, pulse duration, pattern, and more.

Unfortunately, there is no easy way to upgrade the firmware (and thus any of the parameters) on this microcontroller from the host computer over USB: the STM8 does not even connect to the main MCU of the HDK that provides the USB interface. However, the IR board has through-holes for a standard debug/programming header, and the programming tool used to flash a new firmware on the STM8 is readily available and inexpensive, so if you aren't afraid to solder (which you'll need to do unless you buy an official, large, round-ish ST/Link v2 programming tool direct from ST Micro and have an HDK 1.4 or 2, in which case you have a cable you can use), it's an achievable task.

## Hardware Bits

See the [Technique: How to add a IR board programming connector to OSVR HDK 1.2 1.3 1.4 2](https://www.ifixit.com/Guide/How+to+add+a+IR+board+programming+connector+to+OSVR+HDK+1.2+1.3+1.4+2/65821) iFixit guide for step-by-step instructions on opening your HDK and adding the programming connector.

You will need an STM8 USB programmer compatible with the ST-Link v2: you can get one like that used in the instructions at the [Sensics OSVR Store](https://osvrstore.com/products/programming-tool-for-hdk-positional-tracking-ir-board).

## Software Bits
On Windows, you'll need to install the [ST-Link v2 drivers](http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link009.html).

Once the driver is installed and both ends of the programmer are connected, you can download and run a firmware upgrade bundle, listed below.
The executable is a self-extracting compressed file: when it opens, just double-click `Program-IRFirmware.cmd` and in a few seconds the programming process will be complete.
A successful run will print messages like this:

```
Basic arguments to stm8flash: -c stlinkv2 -p stm8s003k3
Arguments to stm8flash for program: -c stlinkv2 -p stm8s003k3 -w "C:\Users\Ryan\src\IR-Board-Programmer\program-tmp.hex"
Determine FLASH area
Writing Intel hex file 5236 bytes at 0x8000... OK
Bytes written: 5236
Arguments to stm8flash for eeprom: -c stlinkv2 -p stm8s003k3 -w "C:\Users\Ryan\src\IR-Board-Programmer\eeprom-tmp.hex" -s eeprom
Determine EEPROM area
Writing Intel hex file 85 bytes at 0x4000... OK
Bytes written: 85
Press any key to continue . . .
```

If you see any errors, double check your connections: make sure the order of the pins/pads on the board (3.3V, SWIM, GND, RESET) are connected to the corresponding pins on the programmer.
Verify that you have the driver installed and that there are no errors in Device Manager.
You may also want to consider [updating the ST-Link v2 firmware](http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link005.html).
If you saw an error, once you've double checked those items, you can just re-run `Program-IRFirmware.cmd` and see if it goes better.

Once you're done programming the board, return to the iFixit guide for re-assembly tips.

Note that the IR board does not need to be connected to anything but the programmer for the process to work, but it can be left attached to the rest of the HDK, so you can re-assemble the HDK and just open the faceplate and tilt out the connector for future programming if desired.

## Firmware Downloads

- 4 August 2016: Substantially improves sync with camera firmware version 0007, especially during fast motion, improves range and reliability by masking troublesome beacons.
	- Self-extracting 7z of firmware and tools for Windows: [IR-Board-Programmer-Bundle-20160804.exe](https://resource.osvr.com.s3.amazonaws.com/public_download/IRBoardFirmware/IR-Board-Programmer-Bundle-20160804.exe)
	- Zip of firmware and tools for Windows: [IR-Board-Programmer-Bundle-20160804.zip](https://resource.osvr.com.s3.amazonaws.com/public_download/IRBoardFirmware/IR-Board-Programmer-Bundle-20160804.zip)

## Firmware Tool Details

For those of you who would like to perform the reprogramming from a non-Windows operating system: we haven't made that process smooth yet but it should be feasible. To figure it out, download one of the `.zip` bundles: inside, you'll find:

- the `.hex` file that contains both the program flash and the eeprom for the STM8.
- Windows binaries for the `srecord` tools, which are open source and available on many operating systems.
- Source (slightly modified to reduce verbosity of warning messages) and Windows binaries for the GPL `stm8flash` tool, which should also be available cross-platform.
- The PowerShell script that ties them all together, splitting the single `.hex` into a program hex and an eeprom hex using `scat` from srecord then using `stm8flash` to flash those components individually. Porting this script to `sh` should be eminently doable, it just hasn't been done yet.
