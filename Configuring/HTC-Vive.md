# HTC Vive and Vive PRE

OSVR supports using your HTC Vive and its controllers to use any OSVR software, whether in direct or extended mode, with distortion correction, with full tracking and input support.

The [OSVR-Vive](https://github.com/OSVR/OSVR-Vive) plugin and tools provide for access to the Vive hardware through OSVR.

[Download pre-compiled OSVR-Vive plugin binaries here!](http://access.osvr.com/binary/vive)

## Compatibility
While the drivers should be compatible with all Vive family devices, it has only been tested so far on the HTC Vive  and Vive PRE (not the earlier Developer Edition). If you have one of these early devices, please consider helping out by testing and reporting back how it works!

The OSVR-Vive driver interacts with the SteamVR driver for the Vive (`driver_lighthouse`), so the version of SteamVR matters as well. The pace of releases has slowed since earlier this year, but typically the driver is only compatible with the latest stable release of SteamVR as Valve usually makes incompatible changes to the betas shortly after each new stable. The version, listed below, can be seen in a number of places, including the SteamVR "System Report" dialog.

Known-good stable release, for the current build as of the time of this writing:
- 1495066092  (confirmed as stable on June 8th 2017)

## Setup
You'll need to first make sure the Vive is working well in SteamVR and that you have a room setup, preferably a standing or room-scale calibration, for your space. Then, download the plugin binaries that match your OSVR server in bits.

You'll have files as follows in your extracted download:
- A plugin file - on Windows, this is a `.dll` file in something like `bin/osvr-plugins-0`. Put that file in the same directory of your OSVR server as the other plugins.
- In `bin`, a `ViveDisplayExtractor` tool - copy to the `bin` directory of your OSVR server.
- A sample `osvr_server_config` config file that you'll want to start with (as well as some pre-made display descriptor and mesh files, which we'll be replacing shortly.)

You should be able to run an OSVR application on your Vive HMD in direct mode without having to change any settings in SteamVR. If Vive works in direct mode in SteamVR, but won't open in direct mode in OSVR, make sure SteamVR has exited before starting the OSVR application. On some systems, it may be necessary to disable direct mode in the SteamVR menu before exiting SteamVR.

One time setup: Your Vive needs a custom display descriptor and mesh distortion data file, which can be extracted using the included `ViveDisplayExtractor` tool. Making sure that `ViveDisplayExtractor` is alongside `osvr_server`, that your Vive is plugged in, and that SteamVR has exited, run `ViveDisplayExtractor`.

When successful, you'll see something like:

```
[DisplayExtractor] Writing distortion mesh data file:
C:/Users/Ryan/Desktop/OSVR/bin/displays/HTC_Vive_PRE_meshdata.json

[DisplayExtractor] Writing display descriptor file:
C:/Users/Ryan/Desktop/OSVR/bin/displays/HTC_Vive_PRE.json

[DisplayExtractor] Press enter to quit...
```

**Note**: The resulting configs contain an absolute path to the mesh data, so if you move where your OSVR Server is located, just run `ViveDisplayExtractor` again.

That's all there is to it! You can now use the sample `osvr_server_config.vive.sample.json` as your server config file (rename it to `osvr_server_config.json` to make it the default) and you'll be ready to run OSVR-powered applications on your Vive!

(When you want to run a SteamVR app, no problem: just close the OSVR Server and any OSVR apps running in direct mode.)
