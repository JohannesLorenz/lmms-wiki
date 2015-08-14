### Dependencies

The following dependencies should be met before compiling LMMS:

Required libraries:

* [Qt](http://qt-project.org/) >= 4.3.0 with devel-files (4.4.x recommended)
  * Qt5 requires special instructions, see [Using Qt5](#using-qt5) below.
* [libsndfile](http://www.mega-nerd.com/libsndfile/)
* [FFTW3](http://www.fftw.org/)

Optional, but strongly recommended (with devel-files each):
* [libvorbis](http://xiph.org/vorbis/)
* [libogg](http://xiph.org/ogg/)
* [libsamplerate](http://www.mega-nerd.com/SRC/) (>= 0.1.7)
* [wine](http://www.winehq.org/)
* [libstk](http://www.libstk.net/)
* [libfluidsynth](http://fluidsynth.sourceforge.net/)
* [FLTK](http://www.fltk.org/) (Needed by ZynAddSubFx)
* [JACK](http://jackaudio.org/)
* [SDL](http://www.libsdl.org/)
* [libalsa](http://www.alsa-project.org/)
* [libportaudio](http://www.portaudio.com/)

The last four dependencies on the list are necessary for audio playback. You may provide one or more of those, depending on what sound server(s) you'd like to use with LMMS.

For building, you'll also need:
* [GCC](http://gcc.gnu.org/) with g++
* [CMake](http://www.cmake.org/) (>= 2.4.5 required, >=2.6.0 recommended)

For example, for installing all dependencies on Ubuntu 12.04 (or later) at once, run:
```sh
sudo apt-get build-dep lmms && sudo apt-get install libfltk1.3-dev
```
Or, manually:
```sh
sudo apt-get install build-essential cmake libqt4-dev libsndfile1-dev libfftw3-dev \
libvorbis-dev libogg-dev libasound2-dev libjack-dev libsdl-dev libsamplerate0-dev \
libstk0-dev libfluidsynth-dev portaudio19-dev libfltk1.3-dev wine-dev \
libxinerama-dev libxft-dev libgig-dev git
```

On some Debian based system if you receive `No rule to make target /usr/bin/fluid`, install fluid manually:
```sh
sudo apt-get install fluid
```

On 64 bits systems you may also have to:
```sh
sudo apt-get install libc6-dev-i386 gcc-multilib g++-multilib
```
 (Fixes *"compilation terminated.  winegcc: g++ failed"*)

### Building on Linux

**Note for Ubuntu users:** You may run into problems compiling LMMS on Ubuntu 12.04 or above, as the `ia32-libs` package has been removed from the repositories. See this page for more information: <http://askubuntu.com/questions/107230/what-happened-to-the-ia32-libs-package>

Instructions on compiling and installing LMMS:


1. Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create a new directory which is needed for you build.

    ```sh
    mkdir build
    ```
2. Then configure LMMS with CMake.

    ```sh
    cd build
    cmake ..
    ```

   This will search your system for all the dependencies and will notify you if something's missing.

   **Notes:**
   * On some Debian based systems if VST fails to locate wine-dev, append ` -DWINE_LIBRARY=/usr/lib/i386-linux-gnu/libwine.so`
   * To include debugging symbols usable with tools like gdb and valgrind, pass the debug flag into cmake: ` -DCMAKE_BUILD_TYPE=DEBUG`, or ` -DCMAKE_BUILD_TYPE=RelWithDebInfo` for a good compromise between program optimization and ability to debug.
   * To specify a install directory different from the default one, invoke cmake with the option `-DCMAKE_INSTALL_PREFIX=<install-dir>`.

3. Now compile LMMS: (People with more than one CPU core can use make's `-j<n>` option, with <n> being the number of threads you wish to use, accelerating the process. Otherwise `make` is just fine. )

    ```sh
    make -j2
    ```
4. Run it:
   
   ```sh
   ./lmms
   ```
5. Optionally, install LMMS into the previously specified directory:

    ```sh
    make install
    ```
6. Note: If you have an older version of LMMS installed, you may run into some GUI problems and glitches. This is because LMMS 1.0.0 is no longer compatible with old 0.4.x themes. To solve the problem: 

    * Run LMMS
    * Go to settings and select the folder tab 
    * Clear the "artwork directory" setting so that it's empty 
    * Restart LMMS

##Using Qt5

Requirements:
  * CMake >= 2.8.11 is required for building with Qt5 support.

Steps:
  * In order to build LMMS with Qt5, add the following flag when invoking cmake:

   ```bash
   -DWANT_QT5=ON
   ```

Notes:
  * If your Qt5 installation does not reside in standard installation paths, additionally pass e.g.

   ```bash
   -DCMAKE_PREFIX_PATH=/opt/qt53/
   ```