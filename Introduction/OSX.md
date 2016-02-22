## Notes on OSX Support 

### RenderManager status on OSX

RenderManager compiles and links under OSX using the same CMakeLists.txt
file that is used on other platforms.  The OpenGL include file structure
is different, so there are #defines in the code to let this compile on
the mac.

As of 2/22/2016, only one RenderManager example program runs on the mac
because the mac does not provide a Compatibility profile for OpenGL, but
any application that uses only the Core profile should work on OSX.

