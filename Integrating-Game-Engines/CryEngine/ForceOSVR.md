## CryEngine OSVR Documentation
OSVR is a supported VR platform in [CryEngine](https://www.cryengine.com/). Official documentation can be found [here](http://docs.cryengine.com/display/CEPROG/VR+-+OSVR).

## Force CryEngine to use OSVR
By default, CryEngine will try to automatically detect and activate one of the supported VR SDK's. In some instances, a CryEngine game may crash if it attempts to use another VR platform and fails before attempting to use OSVR. In order to force OSVR only, and skip other platforms, users can add a configuration file named _user.cfg_ to the root directory of the game's build. 

_user.cfg_ should contain the following lines to force OSVR support:
```
sys_vr_support = 1
hmd_driver = 3
```