# Predictive Tracking

## Overview
In the context of AR and VR systems, predictive tracking refers to the process of predicting the future orientation and/or position of an object or body part. This document will cover configuring prediction parameters in OSVR. More information about why prediction is useful, how it works, and common prediction algorithms, see: http://vrguy.blogspot.com/2016/05/understanding-predictive-tracking.html

## Server-side prediction
Server-side prediction is turned on by default for the HDK. To turn off server-side prediction, add the following to the server config:
```json
"aliases": {"/me/head":"/com_osvr_Multiserver/OSVRHackerDevKit0/semantic/hmd"}
```
The path differs for other devices. Use osvr_print_tree.exe to find the unpredicted path for your device.

## Client-side prediction
Enabling both client and server-side prediction at the same time will likely result in poor performance. Client-side prediction works best with server-side prediction disabled (see above), but it only works with Direct Mode enabled in RenderManager. The following JSON enabled client-side prediction in RenderManager:
```json
"prediction": {
					"enabled": true,
					"staticDelayMS": 22,
					"leftEyeDelayMS": 7.5,
					"rightEyeDelayMS": 0,
					"localTimeOverride": true
			},
```
"staticDelayMS" sets how many milliseconds ahead to predict. Adjust this value to balance perceived latency and noise/accuracy.

For HMDs with one physical display, it is often the case that the image for one eye appears with a delay of half a frame relative to the other eye. This is the case for HDK 1.4 or earlier. If each eye has its own display, as is the case with HDK 2.0, set each eye delay to 0.

## Rendering optimzations
For more information on rendering optimizations, see: https://github.com/sensics/OSVR-RenderManager/blob/master/doc/renderingOptimization.md