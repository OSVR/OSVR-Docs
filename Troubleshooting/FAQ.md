# Frequently Asked Questions (FAQ)

Below you will answers to some common problems that users experience when they use OSVR for the first time.

Q. When I plug in HDK, the screen is black and doesn't turn on after plug/unplug multiple times.

A. There are a few things that may be causing the screen to remain black :
	
1. Verify that both USB and HDMI cables are connected to the computer and make sure that HDK cable is connected securely to the beltbox.
2. Your device may be in direct mode (DM). When DM is enabled, the screen will turn off, but once you launch a game, it will turn back on. You will not be able to see the desktop background.

Q. How to enable or disable Direct Mode 

A. Direct Mode allows us to directly send input (games, video) to the headset. Extended mode is equivalent to mirroring your monitor on HDK display (act as a second monitor). Download the latest [OSVR Runtime](www.osvr.github.io/using) and launch OSVR-Central where you can enable and disable Direct Mode.

Q. Every time I launch osvr_server, it exits out immediately. Why is this happening?  

A. There can only be one (highlander) instance of osvr_server running. Check if there is another one already opened (check Task Manager for running apps).

Q. I see the same image in two eyes. How can I fix this?

A. Download and install OSVR Control: http://sensics.com/software/OSVRControl-SW/publish.htm. Make sure your HDK is connected to your computer. Run OSVR Control. Click "connect" in the top right, then click "Toggle side-by-side". 

Q. How can I upgrade firmware of my HDK?

A. Download and install OSVR Control from this [link](http://sensics.com/software/OSVRControl-SW/publish.htm). Then you can refer to this [article](https://github.com/OSVR/OSVR-Docs/blob/master/Utilities/OSVRControl.md) on how to upgrade firmware using OSVR-Control.

Q. Where can I download OSVR?

A. You can download OSVR Runtime, SDK and plugins from our website [OSVR Downloads](www.osvr.github.io/using).