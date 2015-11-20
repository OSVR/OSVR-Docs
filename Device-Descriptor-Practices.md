This is an ad-hoc collection of practices for setting up good semantic alias trees for devices.

## For controller buttons that are labeled, make the semantic alias the label, in lowercase.

e.g.
```json
"a": "button/0",
"b": "button/1",
"x": "button/2",
"y": "button/3",
"start": "button/6",
"back": "button/7"
```

## Give each joystick its own "directory" with a path entry for each axis.

Include the button, if applicable - this means you could theoretically have, in a joystick with twist and a push-down button (a combination I've never seen but which undoubtedly exists somewhere)

```json
"joystick": {
    "x": "analog/0",
    "y": "analog/1",
    "rotate": "analog/2",
    "button": "button/8"
}
```

## For Point-of-View "hat" inputs, have an entry named "hat" that points to the heading, and nest the buttons, if available, inside.

Heading values are analog, and conventionally -1 if no input/centered, 0 for north/up, 45 for northeast/up and right, etc.

```json
"hat": {
    "$target": "analog/4",
    "up": "button/12",
    "right": "button/13",
    "down": "button/14",
    "left": "button/15"
}
```

## If your device takes two hands to operate or is otherwise not intended for multiple use, suggest automatic alias to `controller`

That is, if you're configuring a generic gamepad or joystick, you probably want this general format in your descriptor:

```json
"semantic": {
    /* your semantic aliases here */
},
"automaticAliases": {
    "/controller": "semantic/*"
}
```

The `automaticAliases` section shown in that example will recursively mirror the semantic tree of your plugin under the "global" `/controller` path, if the user's path tree does not already have such aliases.

This should **not** be done if your controller is explicitly single-handed or might be used with one in each hand - in that case, let the user/configurator set up the appropriate global aliases.

## Check similar devices' descriptors for reference

While not all descriptors are guaranteed to be of the highest theoretical purity, they do give an indication of existing practice, especially when multiple descriptors are considered.

See <http://osvr.github.io/compatibility/> for a syndicated/aggregated list of device compatibility that includes links to the device descriptors associated with each entry.