# Positional Tracking
In February 2017, an updated video-based tracker plugin was included in OSVR-Core (version **0.6.1935-ga2cba4b6 and later**). Because it 

**Updated 2-Feb: You can now download an [OSVR Runtime Installer](http://access.osvr.com/binary/osvr-runtime-installer) that includes the plugin - you want version 0.6.1935-ga2cba4b6**

Yes, it will still print an "experimental" warning on startup in the first build (post-v0.6-1934 the warning is removed), and some use cases might not be ideally tuned, but it is a substantial improvement over the previous plugin, and it made sense to integrate this for wider usage now rather than wait any longer to get it "perfect" given that we had expected to have it completed much before now. Sorry for the delay; tracking isn't a simple problem, and the perfect is the enemy of the good.

The updated tracker code does a lot of things better than the old code: I summarize a few of them below in the course of the notes, but in general it should be more reliable and more stable, on both HDK 1.x and HDK 2. It's also far more featureful and extensible.

**Note that there is an upcoming refinement build based on feedback here which will have a version number later than v0.6-1934 - it makes some of the notes below non-issues. I've tried to update accordingly, and will keep updating as new builds roll out.**

## Config files

For compatibility, and because the new tracking system is actually an entirely independent plugin from the old one (which was actually two plugins configured to work together), there are **new sample config files to use if you want to use the new tracking, known as the "UnifiedVideoTracker"**.  They are:

- `osvr_server_config.UnifiedVideoTracker.HDK2NotUpgradedDirect.json`
  - For HDK2 units that were *manufactured as HDK2* (not upgraded with the upgrade kit) - in direct mode. (HDK 2 chassis has slightly different IR beacon positions from 1.x, so this config has a line to indicate it should choose the HDK2 beacon pattern.)
  - Modify as needed for extended mode.
- `osvr_server_config.UnifiedVideoTracker.HDK2UpgradeKitDirect.json`
  - One of the rare places where a manufactured HDK 2 differs from one made with an upgrade kit: this config file is for upgrade-kit HDK 2 units that were originally HDK 1.3/1.4. It has HDK 2 display configuration, but HDK 1.x beacon locations.
  - Modify as needed for extended mode.
- `osvr_server_config.UnifiedVideoTracker.HDK13DirectPortrait.json`
  - for HDK 1.3/1.4 in direct mode in portrait (ideal).
  - Modify as needed for extended mode, see other config for landscape.
- `osvr_server_config.UnifiedVideoTracker.HDK13DirectLandscape.json`
  - for HDK 1.3/1.4 in direct mode in landscape.
  - Typically you'd only use landscape direct mode on a 1.x if you're using an AMD card - their drivers don't appear to like the portrait display timings. Other reasons to use landscape on 1.x would be a wireless transmitter or other HDMI peripheral that won't work with a portrait input. (Landscape mode rotation on an HDK 1.x incurs a 1-frame latency penalty because of the requirements of actually performing the image rotation and scan-out.)
  - Modify as needed for extended mode, see other config for portrait.
- `osvr_server_config.UnifiedVideoTracker.HDK12DirectLandscape.json`
  - for HDK 1.2 in direct mode, landscape video input - as above, you'd leave it landscape likely only if you're using an AMD card, or a wireless transmitter or other HDMI peripheral that won't work with a portrait input. 
  - Can be modified for extended mode or portrait.

## Notes

**Please read at least the bold/italic text as there are important differences/changes you should be aware of.**

### Config and calibration (_updated 1-Feb_)

- While the config files (*in the original build only - removed post-v0.6-1934*) mention calibration files and they can in fact be used, please don't; **do _not_ pre-calibrate unlike with the old tracker plugin**, especially if you are using a manufactured HDK2.
  - The calibration tool is based on the core of the old tracker plugin, which isn't aware of the changed HDK2 beacon locations, so if you make a calibration file it won't be based on the right positions to start with, but will override them instead. You can just delete or rename your calibration file so it's not found to prevent it from loading.
  - While both old and new plugins actually auto-calibrate the beacon locations during runtime, the new plugin does a much more effective job of this, so pre-calibration hasn't been necessary in every test I'm aware of.
  - This also means the calibration tool is an even worse way of looking inside the operation of the tracker than it was before, since it's using the old algorithm - **if you want to see the annotated camera image, just enable the debug view line in the config file.** Small performance hit, so it's off by default, but I know people like to look at this stuff sometimes.
- For optimal functioning of the rear tracking target, **update the head circumference value in the config file** to match your head. The default is somewhat large (it's the circumference of the glass head I use for initial testing, tbh) and will actually work OK with smaller heads too, but the algorithm will have to work less hard to "auto-calibrate" those rear beacons into place if it starts with a better estimate of where they are on your head. Units are centimeters, measured around where the head strap goes. (It's just used to estimate a diameter/distance from the front HMD beacons to the back ones.)

### Default position (_updated 1-Feb_)

- Note that the new tracker has a default location specification for the camera of (0, 1.2, -0.5) - which means that by default, it assumes that the camera is at approximately eye level for a seated position and that the reported position is approximately corresponding to the real world, with the origin (0, 0, 0) below you on the floor. A few implications:
  - **You have to zoom out to see the HMD on tracker viewer - use the scroll wheel.**
  - You might need to change the "cameraPosition" config for some games that were used to having your head at (0, 0, 0) - the default is as if there was this line in "params": `"cameraPosition": [0, 1.2, -0.5]`

### Testing and Latency (_updated 1-Feb_)

- When testing, **test a native OSVR application before trying any SteamVR applications**. The SteamVR plugin doesn't currently convey the velocity reports needed for SteamVR to do prediction, so it will likely have more judder than it should. OSVR native apps using RenderManager correctly use the reported velocities to predict poses to the render time. Yes, a fix to SteamVR-OSVR to resolve this is forthcoming. See my comments in this post for more details.
- The prediction intervals may not be entirely perfectly tuned yet, so if you see lag even in an easy to render OSVR native application, edit (increase) the `"staticDelayMS"` value in one of the following RenderManager config files as appropriate for your HDK. This adjusts the latency compensation time. **Note that v0.6-1934 and later contain updated values here which may resolve lag issues.** (Don't mess with the other delays in these files - they're innate to the HMD hardware and thus well-known.)
  - `renderManager.direct.landscape.HDKv2.0.newtracker.json` for HDK 2
  - `renderManager.direct.portrait.newtracker.json` - for 1.x
  - `renderManager.direct.landscape.newtracker.json` - for 1.x in landscape mode

### In case of unplugging/camera movement

- The new plugin now uses the IMU (orientation tracker) in the same processing step as the camera data. This provides overall improvements to both position and orientation tracking. It does mean, however, that you'll get even more unexpected results than you would have with the old plugin if you unplug/replug the HMD during tracker operation (since that would require the software to re-align the IMU and camera, which it currently only does at startup) or if you move the camera during operation (which is in fact the same problem, just the other half of it, and a good deal harder to detect).
  - **Restart the server** if your HMD comes unplugged or you move the camera. Shouldn't need to restart any applications, though.

### IR board and other firmware (_updated 1-Feb_)

- **This plugin is meant for use with upgraded IR driver board firmware, and may work poorly or not at all with older firmware. Performance on old IR board firmware is not correlated with performance on updated IR board firmware.** This plugin has only been (recently) tested using upgraded IR driver board firmware, which is required to work around sync and exposure issues in the third-party IR camera, as well as to improve the tracking stability by selectively disabling some LEDs. Tests with an older version of this branch on the old driver board firmware indicated that the performance, while theoretically improved, had unpleasant artifacts. So, we only recommend using this if you've upgraded your IR driver board firmware. The firmware update improves reliability and range, and particularly so when used with this new plugin. (Not sure if updated firmware is shipping in new units yet.) More [details in my reply to this comment](https://www.reddit.com/r/OSVR/comments/5rdm6h/updated_tracker_code_now_merged_and_ready_for/dd7goqb/)
  - If you decide to risk using the old firmware with this, please share your experiences in the [*comment thread I started below specifically for that purpose*](https://www.reddit.com/r/OSVR/comments/5rdm6h/updated_tracker_code_now_merged_and_ready_for/dd6fld8/) . Note that old IR board firmware is not recommended for use with either plugin, since the sync issues on the camera can result in loss of tracking then a "snap" as it regains lock if you move too quickly or rotate your head quickly. It would be slightly useful to know if people generally find this plugin better or worse than the old one on old firmware, just for those who cannot update their firmware yet.
- **The plugin will not run at all if your IR camera has outdated firmware.** This shouldn't be an issue for most HDK2 users, and I'm hoping that essentially all users have updated their IR camera by now, but *in case it appears to do nothing, check the console messages for a warning about your firmware version*.
- **The version of the main firmware on the HDK doesn't affect tracking in general.** This version number (something like 1.99), while important for general usage, doesn't really affect the behavior of the tracking unless you've got a really old firmware version that is delaying IMU reports, or if you're on a firmware for HDK2 that doesn't reliably start/stop the display. (in the latter case, see <https://www.reddit.com/r/OSVR/comments/5rc807/another_testing_firmware_for_the_hdk_2_v199dev/> for a firmware that should hopefully fix your problem.)

### HDK 1.3/1.4 "Performance Upgrade" (_updated_ with workaround for those without access to a Windows machine)

- If you're using an HDK 1.x that hasn't been upgraded to an HDK 2, note that this plugin is tuned for usage with the ["performance upgrade"](https://osvrstore.com/collections/software/products/performance-upgrade-for-hdk-1-3-1-4) installed. This is a one time software process, available through the OSVR Store at no charge. You will see the best performance if you've done this process, though it will work without it if you don't have access to a Windows machine to run the tool once (in terms of relative importance to positional tracking, the IR board firmware upgrade is more critical).
  - After installing the performance upgrade, do be sure to update to the latest main HDK firmware - the performance upgrade tool currently installs 1.98 when it's done, but 1.99 is available.
  - If you don't have the performance upgrade, you'll likely see more latency, since the reporting rate is lower, the orientation data that is coming in has some additional latency in it, and angular velocity has more on top of that, compared to post-"performance upgrade". I'd suggest trying the following changes which might help, though note the values are just approximate and haven't been thoroughly tested:
    - `"orientationMicrosecondsOffset": -5000`
    - One of the following:`"useAngularVelocity": false` (disable usage of angular velocity input altogether - doesn't affect angular velocity output) or `"angularVelocityMicrosecondsOffset": -5000` (only works with v0.6-1934 or later)
  - If you come up with values that are better, let us know here or on [GitHub](https://github.com/OSVR/OSVR-Core).
  - Since the Performance Upgrade is not compatible with the HDK 1.2 hardware, builds v0.6-1934 and later come with the above workaround in the 1.2 config file.

### Console Messages (_clarified_)

- The console messages are somewhat different than the old plugins; while I've quieted most of the debug-type messages, some remaining (and useful!) status messages (for troubleshooting or understanding what's going on) might be a bit more technical in phrasing than a general audience might expect.
  - For instance, **"In flight reset - error bounds exceeded" just means the tracker has lost sight of you or something similar, and is not a problem if tracking otherwise works.** Error in that case is actually just talking about statistical distributions and a common part of operation, not that something has gone wrong. It means the tracker knows its incremental "guesses" have gotten increasingly uncertain past a limit we've coded in, usually because it hasn't gotten enough input data (beacons out of sight, etc) to be more confident (that is, reduce the estimated state error). Since this does usually mean that the HMD has gone out of sight, or at least that the position estimates are not very useful, exceeding the limit flips the tracker from "intelligent" filtering mode to "brute force" mode until it sees the HMD for sure and gets a fresh start with a full estimate. It's what keeps you from drifting too far or spinning out of control if you move out of sight.
    - **This is also a message you might see a lot if you are running an old IR board firmware.**
  - **In practice, unless you're having problems, you can ignore the console messages.**
  - If you _are_ having problems, the console messages, as well as a screen capture of debug view turned on, can be very useful.
