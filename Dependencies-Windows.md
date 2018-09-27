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
 * [**Compiling on Windows using MSYS**](#windows-msys)
    * [Setup Shell](#setup-shell)
    * [Setup Basic Build Environment](#setup-basic-build-environment)
    * [Setup Remaining Dependencies](#setup-remaining-dependencies)
    * [Setup Additional Workarounds](#setup-additional-workarounds)
 * [**Compiling on Windows using MSVC**](#windows-msvc)
    * [Prerequisites](#prerequisites)
    * [Installing dependencies](#installing-dependencies)
    * [Compile using Qt Creator](#compile-using-qt-creator)
    * [Compile using the command line](#compile-using-the-command-line)

<br><!-- End Section--><br>

## Ubuntu Cross Compile
Configure a `mingw-w64` cross-compilation environment for building Windows binaries in Ubuntu.

### Setup PPA
```bash
# For Ubuntu 14.04 "Trusty"
sudo add-apt-repository ppa:tobydox/mingw-x-trusty
sudo apt-get update
```

### 32-bit
Using Qt5 (`lmms>=1.2.0`)
```bash
sudo apt-get install cmake mingw32-x-sdl mingw32-x-libvorbis mingw32-x-lame \
mingw32-x-fluidsynth mingw32-x-stk mingw32-x-glib2 mingw32-x-portaudio \
mingw32-x-libsndfile mingw32-x-fftw mingw32-x-flac mingw32-x-fltk \
mingw32-x-libgig mingw32-x-libsamplerate mingw32-x-pkgconfig \
mingw32-x-binutils mingw32-x-gcc mingw32-x-runtime mingw32-x-qt5base \
nsis
```

### 64-bit
Using Qt5 (`lmms>=1.2.0`)
```bash
sudo apt-get install cmake mingw64-x-sdl mingw64-x-libvorbis mingw64-x-lame \
mingw64-x-fluidsynth mingw64-x-stk mingw64-x-glib2 mingw64-x-portaudio \
mingw64-x-libsndfile mingw64-x-fftw mingw64-x-flac mingw64-x-fltk \
mingw64-x-libgig mingw64-x-libsamplerate mingw64-x-pkgconfig \
mingw64-x-binutils mingw64-x-gcc mingw64-x-runtime mingw64-x-libsoundio \
mingw64-x-qt5base mingw32-x-gcc mingw32-x-qt5base nsis
```

### Qt4
If Qt4 is required (`lmms<=1.2.0`)
```
sudo apt-get install mingw32-x-qt mingw64-x-qt
```
### Cross-compile on later version of Ubuntu
If you want to cross-compile LMMS on Ubuntu later than Trusty(ex. Xenial, Artful), you need to install [libisl10](https://packages.ubuntu.com/trusty/libisl10) to resolve dependencies.

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>


## Windows MSYS
Configure a `mingw-w64` environment in Windows using [`msys2`](https://msys2.github.io/) from Start Menu

### Setup Shell

Setup a unix-like shell environment using [`msys2`](https://msys2.github.io/)

First, [download](https://msys2.github.io/), install and launch msys2 from Start Menu (or manually from `msys2_shell.cmd`).

**Important** The msys2 mirrors have reliability problems and this causes packages to fail during download.  At any time setting up dependencies if an error occurs, try the command again.  For longer tasks, [scroll up and look for them](https://cloud.githubusercontent.com/assets/6345473/25561734/60bd3132-2d40-11e7-975b-5723a218b0f7.png).

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
# Fetch list of outdated packages.
# If this errors out, follow instructions carefully and try again.
pacman -Su

# "Errors occurred, no packages were upgraded" is normal, just try again

# Download and install the 32-bit and 64-bit toolchains (about 85MB)
pacman -S mingw-w64-x86_64-gcc mingw-w64-i686-gcc

# Download and install dependencies (about 726MB, 3.3GB installed)
pacman -S git mingw-w64-x86_64-pkg-config make cmake wget p7zip gzip tar binutils mingw-w64-x86_64-qt4 mingw-w64-i686-qt4 gdb diffutils perl-List-MoreUtils perl-XML-Parser

```
Qt5 hasn't been tested and will likely cause problems but can be provided by installing `mingw-w64-x86_64-qt5 mingw-w64-i686-qt5` instead of qt4 packages.

### Setup Remaining Dependencies
Using `Mingw-w64` from Start Menu (or manually via `mingw64.exe`).  

**Important** The following commands **won't work** from msys2 console.  It needs to be mingw!

```bash
# Delete any old helper scripts
rm -f msys_helper.sh

# Download latest msys_helper.sh helper script from master
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

## Windows MSVC
Warning, MSVC building is still experimental.  Use with caution. Most plugins aren't available when compiling with MSVC.

Use the `master` branch, MSVC is not supported on `stable-1.2`.

```bat
git clone -b master https://github.com/LMMS/lmms.git
```

### Prerequisites

1. Install Visual Studio Community from here: https://www.visualstudio.com/downloads/
2. Install Qt Open Source version from here: https://www.qt.io/download
   Select the newest Qt version for MSVC 2017 or MSVC 2015, installing Qt Creator is recommended. If you want to build for 64bit, make sure to install Qt 32bit as well to support 32bit VSTs. Because Qt doesn't offer any 32bit binaries for MSVC 2017, you'll have to install Qt for MSVC 2015 instead. This is fine as MSVC 2017 can safely link against binaries compiled with MSVC 2015.
3. Install CMake from here: https://cmake.org/download/
4. Installl Git

### Installing dependencies

1. Install Vcpkg:
   ```bat
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   ```
2. Install LMMS dependencies using Vcpkg:
   ```
   vcpkg install fftw3 libsamplerate libsndfile sdl2
   ```
   If targeting 64bit, add the option `--triplet x64-windows`.

### Compile using Qt Creator

Skip to the [next section](#compile-using-the-command-line) if you want to use the command line instead.

1. Open up Qt Creator
2. Go to <kbd>Tools</kbd>-><kbd>Options</kbd>->`Build & Run`->`Kits`. Qt Creator should have detected your Qt installation automatically. We need to make adjustments to it so that CMake finds the libraries installed using Vcpkg. Select the detected kit and click <kbd>Change...</kbd> next to "CMake configuration":

   ![](https://user-images.githubusercontent.com/2879917/41245398-36be3486-6da8-11e8-96b9-8ca98b227404.png)

3. Assuming Vcpkg is installed at `C:\vcpkg`, add `;C:\vcpkg\installed\x64-windows` to the line starting with `CMAKE_PREFIX_PATH`:

   ![](https://user-images.githubusercontent.com/2879917/41245528-86437dfe-6da8-11e8-9a10-4c1d898dfbf9.png)

4. Exit the options and open `CMakeLists.txt`.

### Compile using the command line

Configure using CMake. This assumes you are using MSVC2017, installed Qt 5.9.5 MSVC 2015 64bit & 32bit using the default path and installed Vcpkg to `C:\vcpkg`. Change paths accordingly.

```bat
cd lmms
mkdir build
cd build
cmake .. -DCMAKE_PREFIX_PATH=C:\Qt\5.9.5\msvc2015_64;C:\vcpkg\installed\x64-windows -DCMAKE_GENERATOR="Visual Studio 15 2017 Win64"
```

If configuring succeeded, compile using
```bat
cmake --build . --config Release
```

## Troubleshooting

&nbsp;&nbsp;&nbsp;...nothing here yet, want to [add something](dependencies-opensuse/_edit)?