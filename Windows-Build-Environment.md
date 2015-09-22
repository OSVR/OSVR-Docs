**Note**: If you're just building a plugin or client application/engine integration/game, you don't actually need to compile this from source - you can (and are encouraged to) use the binary snapshots. See <http://osvr.github.io> for downloads.

## Typical build environment: Visual Studio 2013
You can use Express for Windows Desktop, or, if you meet the license criteria, Community.

### Method 1: Bare PC
If you're starting from scratch, you might consider using our [DEV Boxstarter installer](http://access.osvr.com/binary/boxstarter), which uses [Boxstarter][] and [Chocolatey][] to quickly install required dependencies (chiefly OpenCV 2.4.x and Boost for actual library dependencies, as well as CMake) as well as recommended tools (Git, notepad++, etc.) You'll still need to install Visual Studio on your own.

[Boxstarter]: http://www.boxstarter.org/
[Chocolatey]: https://chocolatey.org/

### Method 2: Chocolatey
You can use [Chocolatey][] (aka "choco") manually to just install the packages required. The packages that aren't in the main choco repo are hosted on a MyGet repo [osvr-deps](https://www.myget.org/gallery/osvr-deps). Once you install choco (see their website for instructions), run the following commands at an admin `cmd`/PowerShell prompt to add the the extra repo and install the dependencies.

```cmd
choco source add --name="osvr-deps" -s="https://www.myget.org/F/osvr-deps/api/v2"
choco install -y cmake OpenCV
```

and, depending on if you want to build 32-bit or 64-bit, add one or both of the following package names to that last install line or a new one: `boost-x86-msvc2013` (32-bit), `boost-x64-msvc2013` (64-bit)

There are a number of other packages you might consider installing from Chocolatey, the following is just a sampling of ones we've found useful (in many cases, useful enough to add them to the boxstarter installers):

> `git git-credential-winstore 7zip notepadplusplus notepad2-mod python2 powershell4 poshgit nuget.commandline`

**Note**: When installing Git, we recommend either using `choco install -y git -params '"/GitOnlyOnPath /NoAutoCrlf"'` or running `git config --global core.autocrlf false` to avoid having Git mangle line endings. See also <https://help.github.com/articles/dealing-with-line-endings/>
