---

title: Installing OSVR for Windows
heading: Installing OSVR for Windows

---

# Installing OSVR for Windows

## Install precompiled binaries

You may download precompiled binaries of OSVR for Windows from our [snapshots page](http://osvr.github.io/using/).

## Building OSVR from source

1. *Prerequisites.* The prerequisites may be installed using the Boxstarter scripts found in the [OSVR-Boxstarter Repository](<%=repo_url 'OSVR-Boxstarter' %>). If you prefer to install them manually, the list of prerequisites follows:
  * [CMake](https://cmake.org/)
  * [Boost](http://www.boost.org/)
  * [OpenCV](http://opencv.org/)
  * [Python 2](https://www.python.org/)

2. *Acquire the source code.* Check out the source code from the [OSVR-Core repository](<%=repo_url 'OSVR-Core' %>).

3. *Create a build directory.* To keep the source repository clean of temporary and generated build files, create a separate directory to contain the build. We usually create a directory named `build` inside the OSVR-Core directory.

4. *Generate a Microsoft Visual Studio project.* Run CMake to generate a project file for your compuiler. Set the `CMAKE_INSTALL_PREFIX` variable to the location where you would like the OSVR files to be installed.

5. *Compile OSVR.* Navigate to the build directory you created in step 3 and load the project file. Build the `ALL_BUILDS` project and optionally "build" the `INSTALL_FILES` project.

## Next steps

<ul class="arrows">
    <li><a href="http://resource.osvr.com/docs/OSVR-Core/">Peruse the OSVR reference documentation</a></li>
    <li><a href="/doc/unity">Integrate OSVR with Unity</a></li>
    <li><a href="/doc/unreal">Integrate OSVR with Unreal</a></li>
    <li><a href="/doc/client">Integrate OSVR with your own application</a></li>
    <li><a href="/doc/plugin">Write an OSVR plugin</a></li>
</ul>

## Support

If you need further assistance with installing OSVR, email us at [`support@osvr.org`](mailto:support@osvr.org).


