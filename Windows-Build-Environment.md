**Note**: If you're just building a plugin or client application/engine integration/game, you don't actually need to compile this from source - you can (and are encouraged to) use the binary snapshots. See <http://osvr.github.io> for downloads.

## Method 1: Bare PC
If you're starting from scratch, you might consider using our [DEV Boxstarter installer](http://access.osvr.com/binary/boxstarter), which uses [Boxstarter][] and [Chocolatey][] to quickly install required dependencies (chiefly OpenCV and Boost) as well as recommended tools (Git, notepad++, etc.)

## Method 2: Chocolatey
You can use [Chocolatey][] (aka "choco")manually to just install the packages required. The packages that aren't in the main choco repo are hosted on a MyGet repo.


[Boxstarter]: http://www.boxstarter.org/
[Chocolatey]: https://chocolatey.org/