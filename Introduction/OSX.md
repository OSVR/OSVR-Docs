## Notes on OSX Support 

### RenderManager status on OSX

RenderManager compiles and links under OSX using the same CMakeLists.txt
file that is used on other platforms.  The OpenGL include file structure
is different, so there are #defines in the code to let this compile on
the mac.

As of 2/16/2016, RenderManager does not yet run on the mac because the
mac does not provide a Compatibility profile for OpenGL and the code
has not yet been modified to switch from the default Legacy profile to
the Core profile.  There is a
[Github issue](https://github.com/sensics/OSVR-RenderManager/issues/7)
open to deal with this and help from the community providing a pull
request to update this would be appreciated.

