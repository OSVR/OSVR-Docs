# Boost C++ libraries 


  * The DEV Boxstarter package takes care of installing a suitable version of Boost. It uses [these Windows boost binaries](http://sourceforge.net/projects/boost/files/boost-binaries/) which are very nice, official, and also well hidden - the only way I've found to discover that page (besides surfing SourceForge) is by clicking the little "More downloads" link on the Boost homepage

  * [Boost Homepage](http://www.boost.org/) - with documentation online. Note that you can edit the version number in doc URLs to look at past version documentation, in case you're targeting an older version of Boost than the latest.

  * http://theboostcpplibraries.com/ is a freely-available eBook (with a dead-tree version available) on their usage.

## Usage: Internal code in OSVR-Core 

Within OSVR-Core, we use C++11 and a relatively recent version of Boost, including some of the compiled libraries (which are statically linked, at least on Windows). We do this because not reinventing the wheel makes us more productive, and the C APIs we export mean that we can be somewhat pickier in what compiler/libraries are required for the core since they're by default not required by the consumers of the library.

Rather than write outdated, duplicated documentation, you can look at the [CMake build file here](https://github.com/OSVR/OSVR-Core/blob/master/CMakeLists.txt) to find the line starting with ''find_package(Boost'': that will tell you the current requirements.

We're not opposed to using more of the library, especially if it's the header-only stuff. If it's a compiled library, and compelling, that should also be fine. Bumping the minimum required version is OK if needed and if you can show how it impacts compilation requirements on the various required platforms: we're not bent on sticking in the stone age (see: usage of C++11), but we do want to make it easy to compile on platforms people are using.


## Usage: OSVR-Core API Consumers 
Applications using the PluginKit or ClientKit C APIs do not need Boost.

However, for the recommended and provided C++ header-only wrappers around these APIs, some Boost is required. We only use header-only libraries in PluginKit/ClientKit C++ API wrappers, and fairly old, widespread functionality at that. (Think ''shared_ptr'' for instance.) This is because of, and in line with, our decision not to require C++ API consumers to have C++11 available. The C++ API wrappers are header-only, so all the implementation is easily visible, and the C API backs it all, so if someone really wanted to write C++-style code without using Boost, they could do so, but such an old version, and header-only, seems a reasonable request.

We haven't bothered to see how old of Boost actually works with the wrappers, but it should handle "pretty old".