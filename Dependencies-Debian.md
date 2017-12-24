## Qt4

```bash
sudo apt-get install cmake ladspa-sdk libsamplerate0-dev libsdl1.2-dev \
libsndfile1-dev libvorbis-dev libmp3lame-dev libjack-dev libstk0-dev libfltk1.3-dev \
fluid libasound2-dev git libpulse-dev libfluidsynth-dev libfftw3-dev \
portaudio19-dev libgig-dev libwine-dev qt4-qmake libqt4-dev libxcb-keysyms1-dev \
libxcb-util0-dev
```

### VST
Install libraries for VST support
```bash
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install wine-development wine32-development wine64-development \
libwine-development libwine-development:i386 fonts-wine gcc-multilib g++-multilib

# keywords: winegcc: g++ failed
# keywords: fatal error: bits/c++config.h: No such file or directory
# keywords: winegcc: File does not exist: /usr/lib/i386/wine/libwinecrt0.a
```

On Debian i386 systems, also run:
```bash
sudo apt-get install wine32-tools
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

### wine-dev
* If VST fails to locate `wine-dev`, append the following to your cmake command
   ```cmake
   -DWINE_LIBRARY=/usr/lib/i386-linux-gnu/libwine.so
   ```

### 32-bit VST
* To enable VST support on non-i386 systems, append the following to your cmake command
   ```cmake
   -DWANT_VST_NOWINE=True
   ```
* A separately i386 `RemoteVstPlugin` must be manually compiled and deployed to the install directory.