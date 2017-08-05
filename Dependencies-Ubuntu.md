**Note:** For cross-compiling Windows binaries on Linux, please see [Windows Dependencies](dependencies-windows) instead.

## Qt5
```bash
sudo apt-get install build-essential cmake libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libmp3lame-dev libasound2-dev libjack-jackd2-dev \
libsamplerate0-dev libsdl-dev libstk0-dev stk libfluidsynth-dev portaudio19-dev \
libfltk1.3-dev wine-dev libxinerama-dev libxft-dev libgig-dev git qtbase5-dev \
qttools5-dev-tools qttools5-dev
```

## Qt4
```bash
sudo apt-get install build-essential cmake libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libmp3lame-dev libasound2-dev libjack-jackd2-dev \
libsamplerate0-dev libsdl-dev libstk0-dev stk libfluidsynth-dev portaudio19-dev \
libfltk1.3-dev wine-dev libxinerama-dev libxft-dev libgig-dev git libqt4-dev 
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

### The following packages have unmet dependencies
Sometimes `apt-get` will not allow all packages to be installed simultaneously. (e.g. `foo : Depends: bar (=1.0.0) but it is not going to be installed`).  Install them or resolve dependencies individually.
```bash
sudo apt-get install libfluidsynth-dev
sudo apt-get install libjack-jackd2-dev
```

This can also happen if you already have Jack1 installed on your system, and you're trying to install Jack2 development files. You should try installing Jack1 dev files instead:
```bash
sudo apt-get install libjack-dev
```

Both Jack1 and Jack2 work fine with LMMS, for differences between the two [please consult the Jackaudio wiki.](https://github.com/jackaudio/jackaudio.github.com/wiki/Q_difference_jack1_jack2)

### winegcc: g++ failed
### fatal error: bits/c++config.h: No such file or directory
On some 64-bit systems, you may also need to run
```bash
# fixes compilation terminated. winegcc: g++ failed
sudo apt-get install libwine-dev libwine-dev:i386 gcc-multilib g++-multilib
```
### For situations where the package manager has trouble finding `libwine-dev:i386` ensure i386 architecture is enabled in dpkg by:
```
sudo dpkg --add-architecture i386
sudo apt-get update
```
### ia32-libs
Older environments may complain that `ia32-libs` package has been removed.  See [this page](http://askubuntu.com/a/107249/412004) for more information.
### If [Tested and working Vsts](https://lmms.io/documentation/Tested_VSTs)(32-bit) do not load and lmms complains about wine packages (in terminal output) try running
```bash
sudo apt-get install wine32
```