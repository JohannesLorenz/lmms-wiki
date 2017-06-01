**Note:** For cross-compiling Windows binaries on Linux, please see [Windows Dependencies](dependencies-windows) instead.

## Qt5
```bash
sudo apt-get install build-essential cmake libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libasound2-dev libjack-dev libsdl-dev libsamplerate0-dev \
libstk0-dev libfluidsynth-dev portaudio19-dev libfltk1.3-dev wine-dev \
libxinerama-dev libxft-dev libgig-dev git qtbase5-dev qttools5-dev-tools \
qttools5-dev
```

## Qt4
```bash
sudo apt-get install build-essential cmake libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libasound2-dev libjack-dev libsdl-dev libsamplerate0-dev \
libstk0-dev libfluidsynth-dev portaudio19-dev libfltk1.3-dev wine-dev \
libxinerama-dev libxft-dev libgig-dev git libqt4-dev 
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

### The following packages have unmet dependencies
Sometimes `apt-get` will not allow all packages to be installed simultaneously. (e.g. `foo : Depends: bar (=1.0.0) but it is not going to be installed`).  Install them or resolve dependencies individually.

```bash
sudo apt-get install libfluidsynth-dev
sudo apt-get install libjack-dev
```

### winegcc: g++ failed
### fatal error: bits/c++config.h: No such file or directory
On some 64-bit systems, you may also need to run
```bash
# fixes compilation terminated. winegcc: g++ failed
sudo apt-get install libwine-dev libwine-dev:i386 wine32-tools:i386 libc6-dev-i386 gcc-multilib g++-multilib
```

### ia32-libs
Older environments may complain that `ia32-libs` package has been removed.  See [this page](http://askubuntu.com/a/107249/412004) for more information.