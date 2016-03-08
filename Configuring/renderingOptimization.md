# OSVR Rendering Optimization

This document describes aspects of the OSVR RenderManager's display pipeline relating to optimizing rendering performance and reducing latency.

The timing diagram below shows repeated video frames being sent to the screen by the display hardware.

![Vertical Sync](./images/vsync_vs_trackers.png)

Each frame starts displaying at the **present** bar, with the first scan lines being displayed.  (Whether all of the pixels are shown at once or they are shown a line or band at a time depends on the display technology being used.)  The pixels are scanned out and displayed, either a row at a time or all together, until the vertical synchronization (**vsync**) bar, at which time the **vertical blanking interval** begins.

To avoid visible artifacts where the screen appears to tear into two parts, the rendering system must avoid updating the data in the frame buffer except during the vertical blanking interval.  One way to do this is to use **double buffering**, which renders into one buffer (called the *back buffer*) and then swaps the buffers during the vertical blanking interval, making it the *front buffer* that is displayed.  The graphics library always renders into the back buffer and then asks the driver to swap buffers at the next vsync.  It is also possible to use only a single buffer, doing **front-buffer rendering**.  This requires the graphics library to interact more closely with the driver and make sure to render into the output buffer only during the vertical blanking interval.

As of 3/8/2016, RenderManager handles [distortion correction](./distortion.md) and time warp (described below) using a second rendering pass.  The client application renders into a virtual screen, which can be oversized to enable distortion correction and time warp to be applied without causing black borders.  When the application is finished rendering, it hands that buffer to RenderManager, who renders it as a texture using a distortion mesh.  For DirectMode rendering pipelines, this can be done using front-buffer rendering.  If so, it must be synchronized with the the vertical blanking interval to avoid tearing.

![Time Warp](./images/render_distort_tw.png)

The **renderManagerConfig** portion of the OSVR server configuration has several entries that control whether and how this synchronization occurs in rendering pipelines that support DirectMode and where **directModeEnabled** is set to *true*.

* **numBuffers**: This selects between front-buffered rendering (1) or double-buffered rendering (2) for the second (distortion/warp) render pass within RenderManager.  Note that double-buffered rendering in this case does not necessarily cause more latency -- if time warp using maxMsBeforeVsync is active, then the back buffer will be swapped to the front immediately after being rendered to.
* **verticalSyncEnabled**: When using double-buffered rendering and this is set to *true*, this has the underlying hardware wait until vsync to present the current buffer, which will avoid tearing because it will not change the buffer being displayed while it is being scanned out.  @todo What happens when we're after vsync already?
* **verticalSyncBlockRenderingEnabled**: When this is set to *true*, RenderManager will block in its rendering/presentation call and not return until the vertical synchronization event has happened (on some render paths and hardware, it waits until the presentation happens).  @todo Waiting until present is wasting time.

## Time Warp

The timing diagram from above is repeated here for reference.

![Vertical Sync](./images/vsync_vs_trackers.png)

Because the user's head is moving during the time rendering is happening, the image rendered to the screen will be out of date by the time it is presented to the user.  To reduce this artifact, RenderManager implements a **time warp** during its distortion-mapping render pass.  This is done through a texture-coordinate reprojection transform that makes the assumption of a 2-meter flat viewing surface to change the on-screen position of each texture coordinate.  (For rotation about the center of projection, this adjustment is exact; for translation of the head, it is only approximate.)

So that it has the most-recent information, RenderManager requests new tracker reports.  Its behavior is controlled by several settings in the **timeWarp** portion of the OSVR server's renderManagerConfig section.

* **enabled**: Turns on time warp when set to *true*.  If it is false, the images are not adjusted based on new tracker data.
* **asynchronous**: If *enabled* is true this flag are both *true*, this causes a separate rendering thread to be constructed.  When the application presents render buffers to RenderManager (or uses the alternate *Render()* path), they are either shared or copied with this thread.  This thread then repeatedly gets new values from the tracker and renders at maximum frame rate (controlled by the DirectMode and other parameters), warping the image based on the latest tracker reports for each frame.  If the application does not send an update before it is time to render a new frame, the last-presented frame is used, re-warped with new tracker data.
* **maxMsBeforeVsync**:  **Deprecated** This flag is no longer being used.  Instead, time warp will use the **verticalSyncBlockRenderingEnabled** flag to determine if it should wait until a vertical sync has occured after it has done its most-recent tracker reading.

## Optimization

Optimal rendering has a number of criteria, some of which are at odds with one another:

* **Minimum latency**: To reduce the time between reading from a tracker and rendering the scene based on that report (whether predicted or not), the tracker's position should be read as close as possible to the time the image will be presented to the display.  **Approaches**:  (1) Use DirectMode (this will often be even faster in portrait mode than in landscape mode for HMDs because their internal circuitry sometimes buffers a frame in landscape mode and then scans it out later).  (2) Use asynchronous time warp with shared buffers.  (3) Set *verticalSyncBlockRenderingEnabled*.
* **Consistent frame rate**: Especially on Windows, the operating system sometimes puts even high-priority threads on hold pending I/O and other operations, which can cause variability in processing time.  Also, with some graphics drivers, the high-priority asynchronous rendering thread is not able to interrupt an ongoing GPU operation.  Either of these can cause RenderManager to miss a frame (or miss a partial frame, causing tearing) if a delay covers the vertical blanking interval.  **Approach**: Use asynchronous time warp.
* **Consistent latency**: A frame-to-frame variation in the amount of time between reading the tracker and rendering the scene can produce apparent jitter (also called judder) in objects while the user's head is in motion.  **Approaches**:  (1) Use asynchronous time warp.  (2) Use time warp with *verticalSyncBlockRenderingEnabled* to cause the time RenderManager looks for a tracker report to be more consistent.  (3) @todo Implement client-side prediction based on the time until presentation.
* **Scene richness**: To maximize the time available for realistic rendering effects, the system should spend as little time as possible waiting during the RenderManager presentation (due to *verticalSyncBlockRenderingEnabled*) so that more time is available in the main thread for rendering instructions to be queued.  **Approaches**: (1) Use asynchronous time warp (which will be faster if you use it shared buffers because it avoids a texture copy).  (2) Disable *verticalSyncBlockRenderingEnabled*.
* **Avoiding tearing**:  When the visible frame buffer has its content modified during scan-out, different portions of the image use different transforms and the image appears to be torn.  **Approaches**: (1) Set *numBuffers* to 2 and *verticalSyncEnabled* to true in DirectMode.  (2) Set *verticalSyncBlockRenderingEnabled* to true in DirectMode. (3) Use non-DirectMode.
* **Smooth animation**: For objects in the environment that are moving (separate from eye-point motion), it is important that there are the same number of animation frames between each displayed frame, to avoid jitter/judder in their motion.  **Approaches**: (1) Disable asynchronous time warp and reduce rendering time (scene richness) to ensure that a new frame arrives.  (2) Use *verticalSyncBlockRenderingEnabled* to ensure that the scene rendering always starts in synchrony with frame scan-out.
* **CPU efficiency**: Because even sub-millisecond sleeps on Windows can cause arbitary delays, many of the approaches used by RenderManager must busy-wait, which increases processor usage.  **Approaches**: (1) Disable asynchronous time warp.  (2) Set *verticalSyncBlockRenderingEnabled* to false.
* **Memory efficiency**: **Approaches**: (1) Set *numBuffers* to 1.  (2) Disable asynchronous time warp, which either requires the application to double-buffer its textures or requires a copy into an internal RenderManager-handled buffer.

### Default Configuration

In an attempt to maximize the client application's time to render while avoiding rendering artifacts, the default OSVR configuration as of 3/9/2016 will be set as follows:

* Asynchronous time warp enabled.  @todo Describe how to set up for double-buffered client-side rendering to avoid copies.
* Set *numBuffers* to 2.  Set *verticalSyncEnabled* to true.
* Set *verticalSyncBlockRenderingEnabled* to true.

### Useful adjustments

A reduction in GPU memory use (at risk of tearing) can be produced by changing to this configuration:

* Set *numBuffers* to 1.  Set *verticalSyncEnabled* to false.

A reduction in GPU memory use and CPU processing power (at risk of skipped frames) can be produced by changing to this configuration:

* Asynchronous time warp disabled.


