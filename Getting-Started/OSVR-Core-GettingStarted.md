## OSVR-Core and OSVR Server

When using OSVR, you need to run OSVR server. It's responsible for handling all communication between hardware devices and OSVR client applications. Download the latest OSVR-Core binary snapshot from http://osvr.github.io/using/, which includes osvr_server.exe, multiple utilities such as osvr_print_tree.exe, as well as sample server configurations.

Let’s examine the /bin directory of an extracted OSVR-Core binary snahpshot:

![OSVR-Core snapshot bin](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_snapshot_bin.png?raw=true)

**osvr_server.exe** will launch the OSVR Server. By default, it uses **osvr_server_config.json** as a configuration file. Since the configuration file is crucial to enabling functionality in OSVR, we will explore it in more detail later.

First, launch **osvr_server.exe**, if you see this message:

![osvr_server no hmd](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_nohmd.png?raw=true)


Then no hardware has been discovered. This document will assume you’re using an HDK. If you need help with HDK setup, please visit the HDK Unboxing and Starting Guide: https://github.com/OSVR/OSVR-General/wiki/HDK-Unboxing-and-Getting-Started

This message shows that the server has found an HDK:

![osvr_server found HDK](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_foundhdk.png?raw=true)

## Tracker View

The Tracker View utility is helpful for quickly checking if a device is connected to OSVR server. For more information, visit (link).

## OSVR Server Configuration - Adding Positional Tracking and RenderManager 
Want to enable positional tracking? Enable DirectMode rendering support? Use a device plugin? All of this requires some setup in your server configuration file. Many sample configuration files are provided in the sample-configs directory of the Core Snapshot. Some devices, such as the Hacker Development Kit (HDK) and Razer Hydra can be auto-detected and will work with a default (empty) config file.

**osvr_server.exe** can be run from the command line with an optional parameter for the server config file. If no parameter is specified, **osvr_server_config.json** will be used instead. Since the HDK is detected automatically, we don’t need to edit **osvr_server_config.json** in order to obtain tracking data from the HDK. If we want to enable positional tracking, however, we’ll need to edit the config file.

Copy the contents of **osvr_server_config.HDK13DirectMode.sample.json** into **osvr_server_config.json**.

![OSVR Snapshot sample-configs](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_snapshot_sampleconfigs.png?raw=true)

Let’s examine our new **osvr_server_config.json**:

![osvr_server_config](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_config_pos_rm.png?raw=true)

These two lines determine which HMD display to use, and which RenderManager config file to use. If you didn’t want positional tracking, these are the only two lines you need to enable RenderManager with the HDK 1.3.

![osvr_server_config snippet](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_config_pos_rm_snippet.png?raw=true)

The rest of the file deals with video-based tracking and sensor fusion. There are options for showing a camera debug view (impacts game performance if this is running in the background), enabling or disabling the LEDs located on the back of the head-strap, changing head circumference, and selecting a calibration file.

![osvr_server_config positional](https://github.com/OSVR/OSVR-Unity/blob/gettingStartedDocs/images/osvr_server_config_pos.png?raw=true)

## Display Descriptors
We call the files located in the /bin/displays directory "Display Descriptors" -- JSON files which describe the properties of a display. To switch between configurations for OSVR HDK 1.3 and OSVR HDK 1.2, for example, the only change in the server config is referncing OSVR_HDK_1_3.json and OSVR_HDK_1_1.json.