> **Note:**  This tutorial was written for Ubuntu 12.04 x64.  The steps may work on other Debian-based distros with slight adjustments.

### Prerequisites
 1. Download the latest source code per [Accessing git repository](Accessing-git-repository) page

### Installing packages for cross compiling
 1. Add Toby's cross-compiling repo

   ```sh
sudo add-apt-repository ppa:tobydox/mingw
   ```

 > **Note:**  Newer distros may need `trusty` changed to `precise` in 
`/etc/apt/sources.list.d/tobydox-mingw-trusty.list`

 1. Refresh available packages
   ```sh
sudo apt-get update
   ```

 1. Install build requirements

   ```sh
sudo apt-get install build-essential cmake nsis cloog-isl libmpc2 mingw32
   ```
   > **Note:** Newer distros have replaced `libmpc2` with `libmpc3` requiring the download of libmpc2 manually [here]( http://packages.ubuntu.com/saucy/libmpc2) and install using `sudo dpkg -i`

 
 1. Fetch the mingw32 dependencies (700MB)

   ```sh
sudo apt-get install mingw32-x-qt mingw32-x-sdl mingw32-x-libvorbis \
mingw32-x-fluidsynth mingw32-x-stk mingw32-x-glib2 mingw32-x-portaudio \
mingw32-x-libsndfile mingw32-x-fftw mingw32-x-flac mingw32-x-fltk \
mingw32-x-libsamplerate mingw32-x-pkgconfig mingw32-x-binutils \
mingw32-x-gcc
   ```

 1. Optionally, again for mingw64 (500MB)

   ```sh
sudo apt-get install mingw64-x-qt mingw64-x-sdl mingw64-x-libvorbis \
mingw64-x-fluidsynth mingw64-x-stk mingw64-x-glib2 mingw64-x-portaudio \
mingw64-x-libsndfile mingw64-x-fftw mingw64-x-flac mingw64-x-fltk \
mingw64-x-libsamplerate mingw64-x-pkgconfig mingw64-x-binutils \
mingw64-x-gcc
   ```

 1. Prepare the mingw build environment

   ```sh
cd
cd lmms
mkdir build target
cd build
chmod u+x ../build_mingw*
   ```

### Compile

1. Run the mingw build script

   ```sh
../build_mingw32
# Note:  You will receive errors from Cmake (libfluid, sdl not found, etc), try again
../build_mingw32
make
# Note:  If you receive "Error 2" At 42%, try running make again
   ```

### Package

 1. Creates the Windows installer package

   ```sh
make package
   ```
 2. You will receive a message

   ```sh
   CPack: - package: /home/ubuntu/lmms/build/lmms-x.x.x-win32.exe generated.
   ```
 3. You may now distribute your Windows Installer package (25MB)

   ![image](https://cloud.githubusercontent.com/assets/6345473/3217984/64582130-efe5-11e3-975f-d494215fb85b.png)


### 64-bit
 1. Remove previous cmake cache

   ```sh
rm CMakeCache.txt
   ```
 1. Repeat [Compile](#compile) and [Package](#package) steps again for `../build_mingw64`