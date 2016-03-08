## OSVR Server hangs upon startup
- Check your anti-virus software to ensure that OSVR server is allowed

## OSVR Server takes up lots of CPU resources
The OSVR server continously polls the various sensors. When a client (e.g. application) is connected, this happens in a main loop with no delay, and thus could take up a CPU core. 

As of commit 1be6530920d278982ab20f0647c415821a1c2f1b (Jan 21, 2016) to OSVR-Core, when a client is disconnected, the server inserts a 1 mSec yield in the main loop and thus substantially reduces the CPU requirements.

It is possible to configure the server to insert a yield even when a client is connected. This is done through the server JSON file. For instance, the following config fragment...

```json
{
	"server": {
		"sleep": 1
	}
}
```

...would cause a sleep of 1 mSec in the main loop. The reason why this is not the default configuration is that the Windows scheduler "cannot be trusted" to really provide 1 mSec (which is in the definition of the sleep functions used), but more troubling, it can insert unnecessary latency of 10 mSec to as high as 20 mSec.

However, if the OSVR server takes up too much of your CPU, you can work with this parameter.

## Common error messages
*Warning: Have received several video tracker reports without receiving one from the IMU, which shouldn't happen.  Please try disconnecting/reconnecting and restarting the server, and if this re-occurs, double-check your configuration files.*

If you see this message on startup, and you haven't modified your config files (you're using a basic config file for video-based tracking on the HDK), it means that the HDK's internal tracker (IMU), which runs at four times the rate of the video tracker, hasn't sent any reports, but the video-based tracker has seen the HDK for an extended period of time, so the Video-IMU fusion plugin determined the HDK's internal tracker has stopped reporting. **In short, the HDK just needs to be rebooted.** Unplug and replug the power connector, or the wide connector from the belt box, to do the power cycle. It may "just work" without having to restart the server, but if not, just close the server and start it again.

Make sure that you've got the latest firmware on your HDK (see [OSVR Control instructions](../Utilities/OSVRControl.md) for info) to help avoid this issue.
