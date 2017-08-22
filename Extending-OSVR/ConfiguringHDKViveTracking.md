# Configuring OSVR HDK2 and other OSVR-supported HMD with VIVE Puck Tracking

This document outlines how to add room-scale positional tracking to HDK2 (and any other OSVR-compatible HMD) by mounting an HTC VIVE tracking puck and using the Lighthouse base stations and tracking system. 

Hardware Requirements:
- OSVR HDK2 with mount. Sensics has provided 3D files for a Puck mount [here](https://github.com/OSVR/OSVR-Hardware-Accessories/tree/master/HDK-VIVE-Puck-Adapter).
- VIVE Tracking Puck
- Lighthouse base stations
- USB - microUSB cable or SteamVR Wireless dongle. A wireless dongle comes with the Puck, or you can [flash firmware to a Steam Controller wireless USB receiver](https://steamcommunity.com/app/507090/discussions/0/312265473878773762/).


Software Requirements:
- [OSVR-Core build or SDK/Runtime installation](http://access.osvr.com/)
- [OSVR-VIVE plugin build](https://github.com/OSVR/OSVR-VIVE)
- Optionally SteamVR and [SteamVR-OSVR](https://github.com/OSVR/SteamVR-OSVR)

### Why?
There are a few reasons this document could be useful:
- To serve as a working example of how to configure a new head/positional tracker using OSVR semantic paths.
- To demonstrate how easily displays and tracking systems can be swapped quickly
- To demonstrate that the VIVE tracking puck (with a wireless receiver) can be used for HDK head-tracking in SteamVR

### OSVR-VIVE 
[OSVR-VIVE](https://github.com/OSVR/OSVR-VIVE/) is the OSVR plugin for the family of VIVE devices -- HMD, Controllers, and tracking puck. Wireless tracking of puck/controllers requires the VIVE HMD or a wireless receiver. Follow the instructions in the [Readme](https://github.com/OSVR/OSVR-Vive/blob/master/README.md) to install the plugin. Basically, download the latest binaries and copy `com_osvr_VIVE.dll` to "/osvr-core-build/bin/osvr-plugins-0/".

### Semantic Paths
OSVR Developers should understand the concept of **semantic paths** within the OSVR software stack. For a good introduction, review this presentation on the [OSVR-Core Path Tree](https://osvr.github.io/presentations/20150419-osvr-software-framework-path-tree/).
With the server running, you can run `osvr_print_tree.exe` (located next to osvr_server.exe) to print the current path tree.
Every OSVR plugin has a **device descriptor** which factors devices into OSVR interfaces and defines semantic (human-readable) paths to access specific devices. In this experiment, we’ll need the VIVE Puck’s semantic path in order to use it as our head tracker.
### OSVR-VIVE device descriptor (([com_osvr_VIVE.json](https://github.com/OSVR/OSVR-VIVE/blob/master/com_osvr_VIVE.json))):
```json
{
    "deviceVendor": "HTC",
    "deviceName": "VIVE PRE and VIVE Controllers",
    "author": "Georgiy Frolov <gfrolov@sensics.com>",
    "version": 1,
    "lastModified": "",
    "interfaces": {
        "tracker": {
            "count": 4,
            "bounded": true,
            "position": true,
            "orientation": true
        },
        "analog": {
            "count": 7
        },
        "button": {
            "count": 14
        }
    },
    "semantic": {
		"hmd": {
            "$target": "tracker/0",
			"button": "button/0",
            "proximity": "button/1"
		},
		"puck": {
            "$target": "tracker/3"
		},
        "ipd": "analog/0",
        "controller": {
            "left": {
                "$target": "tracker/1",
                "system": "button/2",
                "menu": "button/3",
                "grip": "button/4",
                "trackpad": {
                    "x": "analog/1",
                    "y": "analog/2",
                    "touch": "button/5",
                    "button": "button/6"
                },
                "trigger": {
                    "$target": "analog/3",
                    "button": "button/7"
                }
            },
            "right": {
                "$target": "tracker/2",
                "system": "button/8",
                "menu": "button/9",
                "grip": "button/10",
                "trackpad": {
                    "x": "analog/4",
                    "y": "analog/5",
                    "touch": "button/11",
                    "button": "button/12"
                },
                "trigger": {
                    "$target": "analog/6",
                    "button": "button/13"
                }
            }
        }
    },
    "automaticAliases": {
        "/me/head": "semantic/hmd",
        "/me/puck": "semantic/puck",
        "/me/ipd": "semantic/ipd",
        "/controller": "semantic/controller/*",
        "/me/hands/left": "semantic/controller/left",
        "/me/hands/right": "semantic/controller/right"
    }
}
```

Take the time to review the device descriptor to understand how the VIVE devices are factored into OSVR interfaces. The parts related to the puck are the assignment to tracker 3, and the automatic alias:
```json
"/me/puck": "semantic/puck",
```

### OSVR Server Configuration
Normally for HDK2 with orientation-only tracking, we’d use a configuration file such as [/sample-configs/osvr_server_config.renderManager.HDKv2.0.direct.json](https://github.com/OSVR/OSVR-Core/blob/master/apps/sample-configs/osvr_server_config.renderManager.HDKv2.0.direct.json). 

Take a look at the last line in the configuration file above. The syntax is similar to how we’ll use the tracking puck:
```json
"aliases": {"/me/head":"/com_osvr_Multiserver/OSVRHackerDevKit0/semantic/hmd"}
```
Instead of setting up an alias to the Multiserver plugin, we want to access the VIVE plugin. We saw earlier that there is an automatic alias in the VIVE device descriptor named “/me/puck”. Our new configuration file with puck tracking for the head is:
```json
"aliases": {"/me/head":"/me/puck”}
```
That’s it! We are now able to move the puck around to change the image in our display. We probably have a little extra work to do depending on the puck's expected orientation and how it is mounted to the HDK.
With the server configured this way, any OSVR clients (Unity, Unreal, etc.) requesting poses from /me/head will get poses from /me/puck, which gets poses from /com_osvr_Vive/semantic/puck, which gets poses from /com_osvr_Vive/Vive/tracker/3. 

### Rotating and Translating
Depending on how your puck is mounted, there may be more configuration needed for tracking to appear in the correct orientation. We decided to mount the puck on top of HDK2 with the crown pointing up and microUSB port facing the forehead, like so:

![Puck Mounted HDK2](https://github.com/OSVR/OSVR-Docs/blob/master/Configuring/images/hdk_puck.jpg)

Doing so requires a -90 degree rotation about the x-axis, which is applied in the server config:
```json
"aliases": {
	"/me/head": {
      "rotate": {
        "axis": "x",
        "degrees": -90
      },
      "child": "/me/puck"
    }
}
```

Finally, we want to translate the origin of the tracker so that it's position appears to be at the center of the eyes. It's mounted a few inches above and in-front of the eyes, so we'll need to translate it towards the head and down in the server configuration file:
```json
"aliases": {
	"/me/head": {
      "posttranslate": [0.0, -0.1143, -0.0635],
      "rotate": {
        "axis": "x",
        "degrees": -90
      },
       "child": "/me/puck"
    }
}
```
Note: Units are in meters. OSVR standardizes internally on a right-handed coordinate system, with X to the right and Y up (and thus Z "near" or toward you). The posttranslate values here may need some fine-tuning depending on exact location of the tracking sensor in relation to the center of the eyes. 
For more information on OSVR conventions, see the [OSVR-Core documentation](http://resource.osvr.com/docs/OSVR-Core/TopicWritingClientApplication.html).

All together, our configuration file with VIVE Puck tracking and HDK2 display looks like:
```json
{
	"display": "displays/OSVR_HDK_2_0.json",
	"renderManagerConfig": {
		"meta": {
			"schemaVersion": 1
		},
		"renderManagerConfig": {
			"directModeEnabled": true,
			"directDisplayIndex": 0,
			"directHighPriorityEnabled": true,
			"numBuffers": 2,
			"verticalSyncEnabled": true,
			"verticalSyncBlockRenderingEnabled": true,
			"renderOverfillFactor": 1.2,
			"renderOversampleFactor": 1.0,

			"window": {
				"title": "OSVR",
				"fullScreenEnabled": true,
				"xPosition": 1920,
				"yPosition": 0
			},

			"display": {
				"rotation": 180,
				"bitsPerColor": 8
			},

			"prediction": {
				"enabled": true,
				"staticDelayMS": 26,
				"leftEyeDelayMS": 0,
				"rightEyeDelayMS": 0,
				"localTimeOverride": true
			},

			"timeWarp": {
				"enabled": true,
				"asynchronous": false,
				"maxMsBeforeVSync": 5
			}
		}
	},

	"aliases": {
		"/me/head": {
			"posttranslate": [0.0, -0.1143, -0.0635],
			"rotate": {
				"axis": "x",
				"degrees": -90
			},
			"child": "/me/puck"
		}
	}
}
```
It's really easy to switch the HDK2 display for any other OSVR HMD -- just swap the name of the display:
```json
"display": "displays/OSVR_HDK_2_0.json",
```
could be changed to work with the Sensics dSight display:
```json
"display": "displays/Sensics_dSight_landscape_1input_sbs.json",
```
Reminder that if you're using OSVRTrackerView to visualize tracked devices, only /me/head, /me/hands/left, and /me/hands/right are visualized by default. To see other devices, specify their path in the command line. For example, to launch OSVRTrackerView and see four connected OSVR-VIVE devices (VIVE HMD, Controllers, Puck), launch OSVRTrackerView with the following command:
```c
OSVRTrackerView.exe --pose /me/head /me/hands/left /me/hands/right /me/puck
```
However, if we are aliasing /me/head --> /me/puck like in the server configuration above, we will be able to see the puck tracking by default without this command.

### Using OSVR-VIVE with SteamVR-OSVR
It is possible to use OSVR-VIVE to use the puck as a positional tracker for HDK2, and then play SteamVR games via the SteamVR-OSVR driver. We call it going through the rabbit hole twice. This is an important use case for HMD manufacturers to quickly test new HMDs with VIVE room-scale tracking by simply mounting a puck. We're still working on some issues on this path after the most recent OpenVR/SteamVR changes, but we think you'll need to make sure you have the following settings in C:\Program Files (x86)\Steam\config\steamvr.vrsettings:
```json
{
   "power" : {
      "autoLaunchSteamVROnButtonPress" : false,
      "powerOffOnExit" : false
   },
   "steamvr" : {
   "activateMultipleDrivers": true,
		"requireHmd" : false
	}
```
and enabling the null driver helps steamvr to start more predictably so we want to edit C:\Program Files (x86)\Steam\steamapps\common\SteamVR\resources\settings\default.vrsettings so the first entry looks like

```json
{
	"steamvr" : {
		"requireHmd" : true,
```

To try it, run OSVR server with the configuration file from earlier, making sure tracker viewer can see and use vive tracker for head tracking as well as the vive controllers are all behaving as expected. 

We need osvr server to be able to use all the steamvr tracked devices to create a pose usinng the defined alias's before trying to start steamvr.  Current steamvr builds and recent firmware updates means whenever a system button on a steamvr tracked device is pressed it causes steamvr to start.  We can use `"autoLaunchSteamVROnButtonPress" : false,` and `"powerOffOnExit" : false` to stop steamvr from being started until we tell it to start using the vr icon from steam client on desktop.

Then we can run SteamVR (after installing the latest build of [SteamVR-OSVR](https://github.com/OSVR/SteamVR-OSVR)) and enjoy vr apps using vive tracker for head tracking. 
