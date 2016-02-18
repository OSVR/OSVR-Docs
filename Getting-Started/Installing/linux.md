# Installing OSVR for Linux

## Building OSVR from source

Note: These are fairly generic build instructions that require you to know how to install libraries and their developer packages on your disribution. Full instructions for building OSVR from scratch on Linuxes resembling Ubuntu 14.04 or Debian 8 [can be found here](Linux-Build-Instructions.md)

1. *Prerequisites.* Many of the prerequisites may be available in your Linux distributions repositories. If you prefer to install them manually, the list of prerequisites follows:
  * [CMake](https://cmake.org/)
  * [Boost](http://www.boost.org/)
  * [OpenCV](http://opencv.org/)
  * [Python 2](https://www.python.org/)
  * [libfunctionality](https://github.com/osvr/libfunctionality)
  * [libusb](http://libusb.info)

2. *Acquire the source code.* Check out the source code from the [OSVR-Core repository](<%=repo_url 'OSVR-Core' %>).

3. *Create a build directory.* To keep the source repository clean of temporary and generated build files, create a separate directory to contain the build. We usually create a directory named `build` inside the OSVR-Core directory.

    ```
    $ mkdir build
    $ cd build
    ```

4. *Generate a Makefile.* Run CMake to generate a Makefile. Set the `CMAKE_INSTALL_PREFIX` variable to the location where you would like the OSVR files to be installed:

    ```
    $ cmake .. -DCMAKE_INSTALL_PREFIX=~/osvr
    ```

5. *Compile OSVR.* Navigate to the build directory you created in step 3 and run `make` to build OSVR. Optionally run `make install` to install the OSVR programs, libraries, and sample configuration files.

    ```
    $ make
    $ make install
    ```
