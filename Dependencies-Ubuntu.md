**Note:** For cross-compiling Windows binaries on Linux, please see [Windows Dependencies](dependencies-windows) instead.

## Qt5
```bash
sudo apt-get install build-essential cmake3 libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libmp3lame-dev libasound2-dev libjack-jackd2-dev \
libsamplerate0-dev libsdl-dev libstk0-dev stk libfluidsynth-dev portaudio19-dev \
libfltk1.3-dev libxinerama-dev libxft-dev libgig-dev git perl libxml2-utils \
libxml-perl liblist-moreutils-perl qtbase5-dev qttools5-dev-tools qttools5-dev
```

### VST
Install libraries for VST support
```bash
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install wine-dev libwine-dev libwine-dev:i386 gcc-multilib g++-multilib libqt5x11extras5-dev qtbase5-private-dev libxcb-util0-dev libxcb-keysyms1-dev

# keywords: winegcc: g++ failed
# keywords: fatal error: bits/c++config.h: No such file or directory
# keywords: winegcc: File does not exist: /usr/lib/i386/wine/libwinecrt0.a
```

### Carla

Install libraries for [Carla](http://kxstudio.linuxaudio.org/Applications:Carla) support (Ubuntu 14.04 provided as example)
```bash
sudo add-apt-repository -y ppa:kxstudio-debian/libs
sudo add-apt-repository -y ppa:kxstudio-debian/apps
sudo apt-get update
sudo apt-get install -y carla-git
```

## Qt4
```bash
sudo apt-get install build-essential cmake libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libmp3lame-dev libasound2-dev libjack-jackd2-dev \
libsamplerate0-dev libsdl-dev libstk0-dev stk libfluidsynth-dev portaudio19-dev \
libfltk1.3-dev libxinerama-dev libxft-dev libgig-dev git libqt4-dev 
```

 * Don't forget to install libraries for VST support [here](#vst) and Carla support [here](#carla)

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

### Unmet dependencies
Sometimes `apt-get` will not allow all packages to be installed simultaneously. (e.g. `foo : Depends: bar (=1.0.0) but it is not going to be installed`).  Install them or resolve dependencies individually.
```bash
sudo apt-get install libfluidsynth-dev
sudo apt-get install libjack-jackd2-dev

# keywords: The following packages have unmet dependencies
```

This can also happen if you already have Jack1 installed on your system, and you're trying to install Jack2 development files. You should try installing Jack1 dev files instead:
```bash
sudo apt-get install libjack-dev

# keywords: The following packages have unmet dependencies
```

Both Jack1 and Jack2 work fine with LMMS, for differences between the two [please consult the Jackaudio wiki.](https://github.com/jackaudio/jackaudio.github.com/wiki/Q_difference_jack1_jack2)

### qmake
If `qmake --version` shows an error:
```bash
sudo apt-get install qt5-default

# keywords: qmake: could not find a Qt installation of ''
```


### ia32-libs
Older environments may complain that `ia32-libs` package has been removed.  See [this page](http://askubuntu.com/a/107249/412004) for more information.
```bash
sudo apt-get install wine32

# keywords: ia32-libs package has been removed
```