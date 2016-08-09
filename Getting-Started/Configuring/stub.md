# Managing OSVR Server

### GUI
OSVR Central is easy to use gui that has many useful tools and features that help manage your vr experience.

You will find the .exe for osvr_central in the `bin` directory where osvr software is installed too.  The default location is C:\Program Files\OSVR\Runtime\bin\  


### CLI

To launch server open a command prompt window and navigate to the `bin` directory of where your osvr software is installed and run `osvr_server`.  Doing this from a fresh osvr install will launch the sample-config file "osvr_server_config.HDKversionDirectMode.sample.json" which has been saved as osvr_server_config.json in C:\Program Files\OSVR\Runtime\bin\

You can launch osvr server using different sample-configs or custom configs several ways.  One easy way is to use windows explorer to navigate to the bin directory and make a shortcut for osvr server .exe.  Save where you need to or just use the suggested desktop location. You could rename it Direct.  Make a copy of that shortcut and rename it Extended.   Using windows explorer navigate to the sample-configs folder and copy the "osvr_server_congHDKforyourversionExtendedLandscape.sample.json" and paste into the bin folder.   Next open properties for the Extended shortcut and refer to the "Shortcut" tab and make the target path read like `"C:\Program Files\OSVR\Runtime\bin\osvr_server.exe" osvr_server_config.HDK13ExtendedLandscape.sample.json` 
Doing this would be helpful for many reasons like being able to show what file is actualy being launched as osvr servr config json and it allows easy management of a custom config.  Custom configs can be made by using a text editor like notepad to edit the contents of .json files.


More ...
