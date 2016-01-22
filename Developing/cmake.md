# CMake 

[cmake](http://www.cmake.org/) is an open-source, cross-platform build system and project generator. It's used by OSVR and its dependencies, and is recommended for both client and plugin use.

Version 3.0.2 (included in the Boxstarter packages as of this writing) is recommended as a minimum.

This is a good http://www.cmake.org/cmake/help/v3.1/manual/cmake-buildsystem.7.html|overall, reasonably complete description of CMake usage from the CMake documentation.

Note that if you're writing an OSVR plugin, there is a wrapper function around ''add_library'' that handles default linking, naming, build directories, and install directories: see the example for details. The first argument, however, is a target name just like ''add_library'' so your build system can be extended beyond the example as far as needed.

## Writing Find Modules 

If you're writing a new driver that has to link against external software, you'll need to tell CMake how to find it.  (If you're writing a client, you can probably skip this section unless you'd like to learn more about using CMake)

If you control the external software and/or it's built with CMake, the preferred route is to export targets with a "config"/package file (this is what OSVR-Core does).

  * http://www.cmake.org/cmake/help/v3.1/manual/cmake-packages.7.html - from the CMake documentation
  * https://github.com/OSVR/OSVR-Core/blob/master/CMakeLists.txt - This is the OSVR-Core main build file - at the bottom, you'll find the exported target stuff.

If you don't control the external software, you can write a "Find Module." The style for these has evolved over time, but the latest recommendations are to mimic the behavior of config-file mode closely.

  * [CMake developer documentation on find_modules](http://www.cmake.org/cmake/help/v3.1/manual/cmake-developer.7.html#module-documentation) - this is how you make one.
  * https://github.com/Kitware/CMake/blob/master/Source/Modules/FindJsonCpp.cmake - while this module itself is effectively obsolete (jsoncpp can make its own config files), it is an excellent example for a clean, simple, and modern Find module.

## Other Resources 

  * [How to build software using CMake](http://academic.cleardefinition.com/2012/05/07/how-to-build-software-using-cmake/) - Screencast
  * [CMake manual for 3.0 series](http://www.cmake.org/cmake/help/v3.0/)
  * [CMake tutorial](http://www.cmake.org/cmake-tutorial/) - excerpted from the Mastering CMake book.