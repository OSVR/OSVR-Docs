#Linux Build Instructions

**Note:** These instructions have been created by building OSVR-Core on a bare bones Ubuntu 14.04. The same steps would work on other Linux distributions as well.

### Compiler: Clang or GCC or G++
Either one of those should work. You will need to build jsoncpp and libfunctionality separately from OSVR-Core : see <http://wiki.osvr.com/doku.php?id=startingcore>

### Required tools/libraries:
Unless specified otherwise, you can use the package manager to get the tools mentioned below

- Git
- CMake (3.0 or newer). Package manager had a version 2.x, so you should get the latest version from <http://CMake.org> or from PPA
- OpenCV - `libopencv-dev`
- Boost libraries (1.44 or newer) You don't need all of them but you'll need at least : `libboost1.xx-dev, libboost-thread1.xx-dev (includes required system, date-time and chrono), libboost-program-options1.xx-dev, libboost-filesystem1.xx-dev`) 
	- There is a [bug in Boost 1.58.0](http://lists.boost.org/Archives/boost/2015/05/221933.php) which makes it incompatible. 
- libusb1 - `libusb-1.0-0-dev`

**CMake notes:** If you are new to CMake then read the following guidelines that you should follow to build OSVR-Core and other libraries, otherwise you can skip this section. To build a project with CMake you need to create a build directory inside your project main directory such as OSVR-Core/build. Next from the build directory, run the following CMake command:

`cmake ..` (This calls CMakelist.txt from directory above and tells it to create config files in current build folder folder)

It will configure the project for you and let you know if are missing any libraries. Once configured you can run `make` to build the project

### Build these libraries first

Once you have the tools above, you can build the following libraries before you begin building OSVR-Core.

- libfunctionality library

`git clone --recursive https://github.com/OSVR/libfunctionality.git`

- jsoncpp library. (You need to run CMake with flags below)  

`git clone --recursive https://github.com/VRPN/jsoncpp`

`cmake .. -DJSONCPP_WITH_CMAKE_PACKAGE=ON -DJSONCPP_LIB_BUILD_SHARED=OFF -DCMAKE_CXX_FLAGS=-fPIC`  

Don't forget `make install` after building jsoncpp.

This should be all the libs/tools that you will need to build OSVR-Core on Linux. CMake messages are very descriptive, if you are missing something, just install it and run `cmake ..` again

### Ready to build OSVR-Core

To get OSVR-Core you'll need to clone it from the main repo (don't forget `--recursive` flag to get all submodules)

`git clone --recursive https://github.com/OSVR/OSVR-Core.git`

Finally just `make`, and watch it build (or grab a soda and read through more [OSVR tutorials], like how to build apps with Unity or Unreal)

[OSVR tutorials]:http://osvr.github.io/build-with/