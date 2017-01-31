**Note**: If you're just building a plugin or client application/engine integration/game, you don't actually need to compile this from source - you can (and are encouraged to) use the binary snapshots. See <http://osvr.github.io> for downloads.

## Typical compiler: Visual Studio 2013
You can use Express for Windows Desktop, or, if you meet the license criteria, Community. No matter which method you choose, you'll need to install jsoncpp and libfunctionality on your own: for pre-compiled Visual Studio binaries of these libraries, see <http://access.osvr.com/binary/deps>  (for jsoncpp, you're free to choose either jsoncpp or jsoncpp-0yz - the former is what is currently used in shipping snapshots, but the latter is considered more updated and reliable.)

Make sure you're using the latest update to Visual Studio 2013. (If you are choosing to use Visual Studio 2015 instead, you'll need at least update 2, and the OpenCV and Boost dependencies below won't work for you - it is quite possible to build with VS2015.2 and 2015.3, but it's not as smooth yet.)

### Preferred Method: Chocolatey
You can use [Chocolatey][] (aka "choco") to install the dependencies/packages required. The packages that aren't in the main choco repo are hosted on a MyGet repo [osvr-deps](https://www.myget.org/gallery/osvr-deps). Once you install choco (see their website for instructions), run the following commands at an admin `cmd`/PowerShell prompt to add the the extra repo and install the dependencies. (The second line installs CMake (3.0 or newer required) and OpenCV 2.4.x binaries into the Choco directory structure - not strictly required but needed for imaging examples as well as video-based "positional" tracking - modify as desired)

[Chocolatey]: https://chocolatey.org/

```cmd
choco source add --name="osvr-deps" -s="https://www.myget.org/F/osvr-deps/"
choco install -s osvr-deps -y cmake
choco install -s osvr-deps -y OpenCV
choco pin add -name OpenCV
```

and, depending on if you want to build 32-bit or 64-bit, add one or both of the following packages: `boost-x86-msvc2013` (32-bit), `boost-x64-msvc2013` (64-bit).  i.e. :
```cmd
choco install -s osvr-deps -y boostx64-msvc2013
```
On your own, you'll want to make sure you have Git, a proper notepad replacement like notepad++ or notepad2mod, etc.

## Alternate build environment: MSYS2/MinGW64
While we don't distribute binaries built with GCC from MSYS2/MinGW64, it's often a useful test of code quality: having more compilers build the same code tends to root out problems and questionable code.

First, download and run the MSYS2 installer: <http://msys2.github.io/> and follow the instructions on that page to update it.

Then, you'll want to install the packages for the build - the command to install packages in MSYS2 is `pacman -s [yourpackagenames]` . Not all of these are strictly necessary (some are optional deps) but it'll get you started. For a 32-bit-build, replace `x86_64` with `i686`.  All you need after this is libfunctionality, which you'll have to build from source (sorry). You'll be able to then open a MinGW 32 or 64-bit shell (accordingly with your choice) and run `cmake` using either the `MSYS Makefiles` generator or the `Ninja` generator, the latter being much faster.

```
mingw-w64-x86_64-SDL2
mingw-w64-x86_64-boost
mingw-w64-x86_64-cmake
mingw-w64-x86_64-gcc
mingw-w64-x86_64-glew
mingw-w64-x86_64-jsoncpp
mingw-w64-x86_64-make
mingw-w64-x86_64-ninja
mingw-w64-x86_64-opencv
```

## Other dev tools

### Clang-Format
You will want to install `clang-format` (and let it put itself on your path): it comes with [Windows binary snapshots of LLVM](http://llvm.org/builds/)  (Get the normal installer, not the 64-bit version). Periodically update this, though note that the format might slightly change between releases so don't be surprised if it suggests changes to files you recently formatted when you update.

If you're using VS Community or one of the paid VS editions, install the clang-format plugin for Visual Studio from the same place. This will let you type the chord `Ctrl-R, Ctrl-F` to format the selected lines right in VS, which is handy.

You can add a shortcut to `clang-format` individual files using `Send to` command on Windows. Open `run` and type in `shell:sendto` which will open a folder with current `Send to` menu items or go to `C:/Users/USERNAME/AppData/Roaming/Microsoft/Windows/SendTo` . Either copy and paste `format-file.cmd` file from `OSVR-Core/devtools` to `SendTo` folder or create a shortcut to that script and copy to that folder. Then you can right click on individual files and `Send to -> format-file.cmd` to clang-format that file.

### Other useful Choco packages

There are a number of other packages you might consider installing from Chocolatey, the following is just a sampling of ones we've found useful (in many cases, useful enough to add them to the boxstarter installers):

> `git git-credential-winstore 7zip notepadplusplus notepad2-mod python2 powershell4 poshgit nuget.commandline`

**Note**: When installing Git, we recommend either using `choco install -y git -params '"/GitOnlyOnPath /NoAutoCrlf"'` or running `git config --global core.autocrlf false` to avoid having Git mangle line endings. See also <https://help.github.com/articles/dealing-with-line-endings/>
