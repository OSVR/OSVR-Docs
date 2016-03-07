
# Oculus Rift plugin

The Oculus Rift plugin provides access to Oculus Rift trackers from OSVR applications. It makes use of the [Oculus VR SDK][https://developer.oculus.com/]. This plugin currently supports Oculus VR SDK versions 0.4.4-beta through 0.8.0.0-beta.

## Installing the Oculus Rift plugin

You may either [download the plugin](http://access.osvr.com/binary/oculus) for your version of the Oculus SDK or [build it yourself](https://github.com/osvr/OSVR-Oculus-Rift#readme).

Copy the resulting plugin file to the `osvr-plugins-0` directory.

## Using the Oculus Rift plugin

First, ensure that the Oculus runtime service is running.

Start the OSVR server using the provided `osvr_server_config.oculusrift.sample.json` configuration file or add `"display": "displays/Oculus_Rift_DK2.json"` to your current server configuration file.


