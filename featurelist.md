# Key OSVR Features

# open-source
- Free and open-source
- Apache 2.0 license
- Supports closed-source plugins

# Universal device support
OSVR supports over 100 devices. The OSVR philosophy is to define an **interface**, essentially a pipe of data that provides reports of a certain type. The following interface types are available:
- Display
- Analog
- Button
- Config
- Eye tracker
- Imager
- Gesture
- Locomotion
- Skeleton
- Poser

Just like a multi-function printer can be viewed by Windows as one physical device that has multiple software interfaces (e.g. printer, fax, scanner), a physical device such as a Razer Hydra can map into one or more OSVR interfaces.

# Semantic Paths

# High-performance rendering support
- Time warp
- Direct Render
- Distortion correction
- Predictive tracking

# Plugin architecture
- Plugins loaded at runtime
- Supports both open-source and closed-source plugin

# Portable
- Cross-platform code that can compile on multiple operating systems: Windows, Linux, Android, OS X and others

# Programming model
OSVR supports both:
- Synchronous (blocking) calls
- Asynchronous (callback)

# Distributed
- OSVR can run on a single machine, but can also be distributed over a network. This allows many-to-many relationship between OSVR clients (applications) and OSVR servers (sensor processors). Clients and servers may also reside on different operating systems.
