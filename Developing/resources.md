# Development Resources

## Editors

General: http://notepad-plus-plus.org/|Notepad++ is the recommended general-purpose editor on Windows. (Don't use regular Notepad, it won't handle the *nix standard line endings.) On Linux or Mac, use your favorite text editor.

## JSON

A recurring theme here will be comments: while they're not technically part of the JSON spec anymore, they're still supported by many tools, including the jsoncpp library used in the OSVR core. (Note, however, that json received by the client app is compacted: extra whitespace removed as well as comments removed, so an app consuming that json doesn't need to use a comment-supporting library)

  * http://jsoneditoronline.org/ (has a Chrome app extension) is handy, though one catch is that changes between the two sides aren't synced automatically, you must do so manually, and it doesn't handle comments.
  * Your text editor will also work fine here.


  * Going a bit "meta": https://github.com/jdorn/json-editor is a slick javascript component, used in https://github.com/OSVR/OSVR-Device-Descriptor-Editor to generate a web-based editor automatically from a JSON Schema. (Of course, some tweaking is required for an optimal experience, but it's a place to start if you're looking at making an editor)

## Markdown

Lots of OSVR stuff is in Markdown - generally an expanded version similar to GitHub-Flavored Markdown, so that the documents look good on GitHub. They may also be rendered by Doxygen or by other tools (such as Discount), but the common subset is pretty comprehensive. Specifically, the items we require in addition to "base" markdown are:

  * Fenced code blocks delimited at top and bottom with ''```'' and labeled with the language.

### Help 

  * [Markdown basics](https://help.github.com/articles/markdown-basics/) and [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) describe how to write markdown text.

### Editors/tools 

  * http://cloose.github.io/CuteMarkEd/ is a desktop Markdown editor for Windows and Linux. While you don't need a specific editor for Markdown, this is handy because it provides a live preview and export options (html and pdf)
  * http://sourceforge.net/projects/retext/ is a desktop Markdown editor for Linux that comes packaged with Ubuntu. It provides a nice preview mode.
  * http://word-to-markdown.herokuapp.com/ is an online word to markdown converter.
## Coding 

For all these items, there's a further wiki page if you click on the name.

  * **[CMake](cmake.md)** is a very good build system (generator) - it's used by all the C++ code related to OSVR, and recommended for third-party contributions as well.

  * The **[Boost C++ libraries](boost.md)** are valuable. They're internally used in OSVR-Core (some built libraries statically linked), and also required by the C++ wrappers for PluginKit and ClientKit (header-only libraries used).

# Boxstarter 

[Boxstarter](http://boxstarter.org|Boxstarter) is a handy tool that builds on https://chocolatey.org/|Chocolatey, which is the closest thing Windows has to a Linux-style package manager. As the name suggests, Boxstarter is designed to get a new system fully installed in an automated way.

There are OSVR Boxstarter scripts [maintained on GitHub](https://github.com/OSVR/OSVR-Boxstarter) that get automatically built and uploaded by the CI server. We bundle Boxstarter and our scripts into self-extracting compressed installers, which are uploaded to http://access.osvr.com/binary/boxstarter.

As of this writing, there are three "flavors" of Boxstarter installer available to download, each designed for a different target audience. Many software packages installed are common between multiple installers.

  * **GENERAL**: This flavor is for those not doing C++ development (for instance, Unity developers) or for easy deployment/config of demo machines. Primarily contains "tools".
  * **DEV**: This flavor is for those doing C++ development, particularly on the core. It includes the external dependencies required to build the core from scratch.
  * **CI**: These are used to prepare Jenkins CI build nodes for the OSVR CI. They might be useful to you in setting up your own CI, though you'll have to change at least the jenkins-updater package.

(The .7z file seen on access.osvr.com is the contents of the installer, but you almost certainly don't want that on its own without the corresponding command line that comes with the installer to launch the right Boxstarter script. It's uploaded for troubleshooting and testing purposes.)

All these packages install Chocolatey locally on your machine, as well as add the package source entries for the re-usable packages installed, so you can use Chocolatey to install other packages or keep things up to date.