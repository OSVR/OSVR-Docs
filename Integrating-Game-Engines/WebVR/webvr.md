#How to use OSVR in Firefox:

1. Download the latest Firefox Nightly build available here - <https://nightly.mozilla.org/>
2. Go to the address bar and type "about:config", which will bring up a list of Firefox settings
3. Search for `dom.vr.osvr.enabled` and set that field to true which will enable OSVR support.
4. Fill in the paths for OSVR libraries (DLLs available both in Runtime and SDK installer) for the following settings:
	1. `gfx.vr.osvr.clientKitLibPath`
	2. `gfx.vr.osvr.clientLibPath`
	3. `gfx.vr.osvr.commonLibPath`
	4. `gfx.vr.osvr.utilLibPath`
5. Start OSVR server with a device of your choice plugged in.
6. Restart Firefox.
7. Check out <https://mozvr.com/> for some of the WebVR demos.

Note: The current implementation supports extended mode.