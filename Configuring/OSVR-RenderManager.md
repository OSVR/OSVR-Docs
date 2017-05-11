## Configuring OSVR-RenderManager

OSVR-RenderManager config defines options that will be used to render to your HMD. Most of OSVR supported HMDs such as OSVR HDK, Sensics dSight, HTC Vive, Oculus Rift and others, already have pre-configured settings that come with OSVR Runtime installer and are optimized for best performance. If you're adding a new HMD, then you can re-use existing RenderManager and display configs to write a new one.

There are two parts to adding RenderManager config :

* `renderManagerConfig` : specifies rendering settings such as enabling/disabling DirectMode, rotation of display, client-side prediction and many others. See RenderManager documentation below for detailed explanations. 
* `display`  : specifies HMD display settings such as resolution, distortion, field of view, and others.

General documentation on OSVR-RenderManager that describes available config options is available here - [RenderManager Documentation](https://github.com/sensics/OSVR-RenderManager/blob/master/doc/renderManagerConfig.md)

#### Dual screen displays / Two video inputs
For dual screen displays HMDs such as [Sensics dSight](http://sensics.com/portfolio-posts/dsight/), which can be configured with 2 video inputs, there are additional instructions on orienting rendering picture.
While some HMDs like dSight accept both portrait and landscape video, other HMDs may only accept portrait mode video, and you will need to specify display rotation in the config to get proper aspect ratio.
In OSVR Server config for your HMDs, you will need to specify the correct resolution for your displays. Under `display` config, make sure to set `resolutions` to landscape (For example if your display is `1440 x 2560` , set the `resolutions` in display config as `2560 x 1440 ` for width and height accordingly), and set display rotation setting in RenderManager config to `90` or `270` degrees. Also specify the number of `video_inputs` and `num_displays`, and set `display_mode: full_screen`.

It is required because rendering happens in the un-rotated display resolution and then RenderManager rotates it (swapping aspect ratio) when rendering.  
