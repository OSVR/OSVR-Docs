# OSVR interfaces
An interface is a pipe of data. A device exposes one or more interfaces. For instance, a Razer Hydra controller exposes several interfaces:

- Two XYZ position interfaces (one for the left hand, one for the right)
- Two orientation interfaces (one for the left hand, one for the right)
- Two buttons sets, one for each hand
- A set of analog interfaces for the variable-position triggers and joysticks

An interface is an instance of an interface class.
An interface class defines properties that can be set or queried as well as a set of events that the class generates. A property might be the last obtained XYZ position from an XYZ position interface. An event could be the press of a particular button in a button set interface.

Interface class specifications [are here](https://github.com/OSVR/OSVR-Specs-and-Proposals/tree/master/Interface%20Class%20Specifications)

OSVR supports the following types of interfaces:

## Tracker interface

This interface represents a device with a 3D position and orientation. For example, the HMD typically exposes a tracker interface for head tracking, and hand trackers like the Razor Hydra expose tracker interfaces for each hand.

Example: /me/head maps to the user's head tracker

Example: /me/hands/left and /me/hands/right map to the user's hand trackers

## Button interface
A general purpose button interface, usable for controllers and other devices with buttons.

Example: /controller/left/1 maps to the left controller's button 1, if using split controllers like the Razor Hydra.

## Analog interface
A general purpose analog value interface. This can be used for any device which provides an analog value. The analog sticks of gamepads are typically exposed as a pair of analog values.

Example: /controller/left/trigger maps to the left controller's trigger analog value.

## Imaging interface
Some VR devices have a built-in camera and can provide a video stream or individual images. For example, the Leap Motion plugin implements the imaging interface and gives applications access to the grayscale IR camera. OSVR also has a plugin that gives you access to the user's webcam.

Example: /camera maps to the user's web cam.

## Eye Tracking interface
This interface allows the developer to see the user's eye position and orientation in 3D space, or as a 2D point on a virtual plane in front of the user (as if looking at a screen). The 2D location interface is shared with other device types that support any kind of 2D location tracking, such a mouse pointer.

Example: /me/eyes maps to the user's eye tracker.

## Locomotion interface
This interface exposes devices such as the Virtuix Omni or a treadmill to provide relative movement on a 2D plane in world space.

Example: /me/feet/both maps to the user's default ## locomotion interface.

## Skeletal tracking interface (draft spec)
This interface supports skeletal trackers that support just the hands (such as the Leap Motion) all the way up to full body skeletal trackers (Kinect and others).

Example: /me/skeleton maps to the user's full-body skeleton.

## Gesture interface
Some devices can provide gesture recognition. This interface supports those types of devices and supports gestures such as swipe left/right/up/down, tap/double-tap, pinch/zoom, open/closed hand and so on.

## Poser interface
This is sort of a reverse tracker interface. A chair that moves into a particular position or orientation can be manipulated with the Poser interface. An old-style arcade machine with moveable seat might use this interface to control the seat movement.
