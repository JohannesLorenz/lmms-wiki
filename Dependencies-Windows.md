## Background
 * The `mingw-w64` cross-compiler is required for building a Windows binary, even on Windows.
 * Cross-compiled dependencies are available via [Ubuntu PPA](#setup-ppa), or may be compiled manually or [using a helper script](#setup-remaining-dependencies)
 * Steps are provided for [Ubuntu Linux](#ubuntu-cross-compile) and [Windows OS](#windows-os)

## Contents
 * [**Cross-compiling on Ubuntu**](#ubuntu-cross-compile)
    * [Setup Ubuntu PPA Mirrors](#setup-ppa)
    * [Setup 32-bit Dependencies](#32-bit)
    * [Setup 64-bit Dependencies](#64-bit)
    * [Setup Qt4 (Optional)](#qt4)
 * [**Compiling on Windows OS**](#windows-os)
    * [Setup Shell](#setup-shell)
    * [Setup Basic Build Environment](#setup-basic-build-environment)
    * [Setup Remaining Dependencies](#setup-remaining-dependencies)
    * [Setup Additional Workarounds](#setup-additional-workarounds)

<br><!-- End Section--><br>

## Ubuntu Cross Compile
Configure a `mingw-w64` cross-compilation environment for building Windows binaries in Ubuntu.

### Setup PPA
```bash
# For Ubuntu 12.04 "Precise"
sudo add-apt-repository ppa:tobydox/mingw-x-precise

# For Ubuntu 14.04 "Trusty"
sudo add-apt-repository ppa:tobydox/mingw-x-trusty
```

### 32-bit
Using Qt5 (`lmms>=1.2.0`)
```
sudo apt-get install mingw32-x-sdl mingw32-x-libvorbis \
mingw32-x-fluidsynth mingw32-x-stk mingw32-x-glib2 mingw32-x-portaudio \
mingw32-x-libsndfile mingw32-x-fftw mingw32-x-flac mingw32-x-fltk \
mingw32-x-libgig mingw32-x-libsamplerate mingw32-x-pkgconfig \
mingw32-x-binutils mingw32-x-gcc mingw32-x-runtime mingw32-x-qt5base
```

### 64-bit
Using Qt5 (`lmms>=1.2.0`)
```
sudo apt-get install mingw64-x-sdl mingw64-x-libvorbis \
mingw64-x-fluidsynth mingw64-x-stk mingw64-x-glib2 mingw64-x-portaudio \
mingw64-x-libsndfile mingw64-x-fftw mingw64-x-flac mingw64-x-fltk \
mingw64-x-libgig mingw64-x-libsamplerate mingw64-x-pkgconfig \
mingw64-x-binutils mingw64-x-gcc mingw64-x-runtime mingw64-x-libsoundio \
mingw64-x-qt5base
```

### Qt4
If Qt4 is required (`lmms<=1.1.0`)
```
sudo apt-get install mingw32-x-qt mingw64-x-qt
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>


## Windows OS
Configure a `mingw-w64` environment in Windows using [`msys2`](https://msys2.github.io/) from Start Menu

### Setup Shell

Setup a unix-like shell environment using [`msys2`](https://msys2.github.io/)

```bash
# From msys2 desktop application, fetch all available packages
pacman -Sy

# Update essential utilities
pacman --needed -S bash pacman pacman-mirrors msys2-runtime

# Restart msys2 (mandatory)
```

### Setup Basic Build Environment
Using `msys2` from Start Menu

```bash
# Fetch list of outdated packages
pacman -Su

# "Errors occurred, no packages were upgraded" is normal, just try again

# Download and install the 32-bit and 64-bit toolchains (about 85MB)
pacman -S mingw-w64-x86_64-gcc mingw-w64-i686-gcc

# Download and install dependencies (about 726MB, 3.3GB installed)
pacman -S git pkgconfig make cmake wget p7zip gzip tar binutils mingw-w64-x86_64-qt4 mingw-w64-i686-qt4 gdb

```

### Setup Remaining Dependencies
Using `Mingw-w64` from Start Menu

```bash
# Download msys_helper.sh helper script
wget https://raw.githubusercontent.com/lmms/lmms/master/cmake/msys/msys_helper.sh --no-check-certificate

# Run the helper script. This will automatically:
# - Download/extract/install the Ubuntu mingw ppa (400MB)
# - Download/compile any conflicting libraries
# - Configure git for use with msys
./msys_helper.sh

# "cp: cannot create regular file" is normal, please ignore

# There will be warnings during library compilations, please ignore
```

### Setup Additional Workarounds
From `cmd.exe`, as Administrator
```bash
# Create symlinks, moc.exe work-around
# - Adjust paths if msys2 was installed in non-standard location
mklink /d %SystemDrive%\mingw64 %SystemDrive%\msys64\mingw64
mklink /d %SystemDrive%\mingw32 %SystemDrive%\msys64\mingw32
mklink /d %SystemDrive%\home %SystemDrive%\msys64\home
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

&nbsp;&nbsp;&nbsp;...nothing here yet, want to [add something](dependencies-opensuse/_edit)?