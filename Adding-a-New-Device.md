## Step 0. "Factoring"

Factoring refers to identifying aspects of your device and associating them with existing (ideally) or new (if required) interface classes. Think of your device like a number or polynomial: factoring breaks it down into the aspects that are combined (multiplied) to provide full functionality. It's an important task that affects how easy it is for applications (new and old) to use the capabilities of your device.

Some useful reference material includes https://github.com/vrpn/vrpn/wiki/Writing-a-device-driver#adding-new-capabilities-to-existing-devices

This is hardest for very novel devices, while it may very well be trivial for the vast majority of cases. It's important to resist the urge to add new interface classes for each device and instead try to find a way to factor them into existing classes if possible: not only is it faster, but it also makes the device more useful in existing and new software. If every device has its own "generic interface", then really there are no generic interfaces.

### Factoring, the easy way

Get in touch with the OSVR community: we can help you figure out how best to factor your device into interface classes in a "Device Integration Proposal".

## Writing the device driver itself
All devices, whether or not they will require new interface class(es) or messages added to existing interface classes, will need a driver themselves. For this, we refer you to another document, [Getting Started Writing a Device Plugin](http://wiki.osvr.com/doku.php?id=startingdevice). Note that if you need a new interface class or a new message type, there will obviously not be the API to write against yet for that particular interface/message, but you can still generally make progress in the rest of the plugin and any other interfaces.

## Adding a new interface class or message
This step, before code is written, requires a good deal of discussion. We'll assume that has already taken place and you've got a good interface class specification you think will serve the community well. There are a few basic pieces of this:

- Add methods/types to API headers (ClientKit, PluginKit, and the report and state types in Util)
  - There's one PluginKit method per interface class, plus at least one per message type (a few overloads for convenience are usually provided).
  - There's two ClientKit methods per message type (a callback registration and a state getter), but these are relatively mechanically generated with the preprocessor, so this is simple.
  - You'll need to add a state type and a report type (usually contains a sensor number and the state type) to a Util header.
- Internal implementation in OSVR-Core: For a new interface class, you'll need a new `DeviceComponent` class (see `ImagingComponent` for a good example) to handle serialization and callbacks. To add a message, you'll just need to edit the existing `Component` class (in the case of interfaces that aren't supported natively by VRPN - backward-compatible wrapping of VRPN messages is more complex and beyond the scope of this high-level overview)
- Wire up the PluginKit and ClientKit APIs to the internals you just wrote.
- Add an example application for display the data, and an example plugin showing how to transmit it.