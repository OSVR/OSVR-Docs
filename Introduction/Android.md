## Notes on Android Support 
### What does it mean to have OSVR support for Android?
OSVR support for Android means that you can run OSVR on Android devices such as:

- Android phones
- Android tablets
- [Razer Forge](http://www.razerzone.com/gaming-systems/razer-forge-tv) game console
- Other types of hardware that supports Android

### Can I use OSVR to run Google Cardboard applications?

Not as-is. Applications need to be ported to take advantage of the OSVR software capabilities

### Can I connect the OSVR HDK to an Android device?

Yes. Users have successfully run OSVR on a Razer Forge and then connected the Forge via the HDMI output and USB port to the OSVR HDK. The same could be done with other HMDs.

### Can I run OSVR on a Samsung Gear VR?

Yes. For instance, see [this video](https://www.facebook.com/Reviatech/videos/892157687538115/?fallback=1) for a user that used OSVR to get positional tracking support for Gear VR.

### RenderManager support on Android

As of 2/22/2106, the RenderManager code compiles and links for Android in a
branch of the [OSVR-Android-Build project](https://github.com/OSVR/OSVR-Android-Build).
There is not yet a working GLES example program to compile, and the code has
not yet been tested.

