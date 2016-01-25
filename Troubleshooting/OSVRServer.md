## OSVR Server hangs upon startup
- Check your anti-virus software to ensure that OSVR server is allowed

## OSVR Server takes up lots of CPU resources
The OSVR server continously polls the various sensors. When a client (e.g. application) is connected, this happens in a main loop with no delay, and thus could take up a CPU core. 

As of commit 1be6530920d278982ab20f0647c415821a1c2f1b (Jan 21, 2016) to OSVR-Core, when a client is disconnected, the server inserts a 1 mSec yield in the main loop and thus substantially reduces the CPU requirements.

It is possible to configure the server to insert a yield even when a client is connected. This is done through the server JSON file. For instance, the following config fragment...

**{
	"server": {
		"sleep": 1
	}
}**

...would cause a sleep of 1 mSec in the main loop. The reason why this is not the default configuration is that we discovered that the Windows schedule "cannot be trusted" to really provide 1 mSec. Often, it is 1 Msec but sometimes it has as high as 20 mSec, inserting unnecessary latency in the application.

However, if the OSVR server takes up too much of your CPU, you can work with this parameter.