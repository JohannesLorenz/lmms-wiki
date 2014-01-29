### Dependencies

The following dependencies should be met before compiling LMMS:

Required libraries:

* [Qt](http://qt-project.org/) >= 4.3.0 with devel-files (4.4.x recommended)
* [libsndfile](http://www.mega-nerd.com/libsndfile/)
* [FFTW3](http://www.fftw.org/)

Optional, but strongly recommended (with devel-files each):
* [libvorbis](http://xiph.org/vorbis/)
* [libogg](http://xiph.org/ogg/)
* [libsamplerate](http://www.mega-nerd.com/SRC/) (>= 0.1.7)
* [wine](http://www.winehq.org/)
* [libstk](http://www.libstk.net/)
* [libfluidsynth](http://fluidsynth.sourceforge.net/)
* [libfreetype](http://www.freetype.org/) (Needed by ZynAddSubFx)
* [JACK](http://jackaudio.org/)
* [SDL](http://www.libsdl.org/)
* [libalsa](http://www.alsa-project.org/)
* [libportaudio](http://www.portaudio.com/)

The last four dependencies on the list are necessary for audio playback. You may provide one or more of those, depending on what sound server(s) you'd like to use with LMMS.

For building, you'll also need:
* [GCC](http://gcc.gnu.org/) with g++
* [CMake](http://www.cmake.org/) (>= 2.4.5 required, >=2.6.0 recommended)

For example, for installing all dependencies on Ubuntu 12.04 (or later) at once, run:
```
sudo apt-get build-dep lmms
```
Or, manually:
```
sudo apt-get install build-essential cmake libqt4-dev libsndfile-dev fftw3-dev \
libvorbis-dev libogg-dev libasound2-dev libjack-dev libsdl-dev libsamplerate0-dev libstk0-dev \
libfluidsynth-dev portaudio19-dev libfreetype6-dev wine-dev
```
On 64 bits systems you may also have to `sudo apt-get install libc6-dev-i386 gcc-multilib g++-multilib`.

### Building on Linux

**Note for Ubuntu users:** You may run into problems compiling LMMS on Ubuntu 12.04 or above, as the `ia32-libs` package has been removed from the repositories. See this page for more information: <http://askubuntu.com/questions/107230/what-happened-to-the-ia32-libs-package>

Instructions on compiling and installing LMMS:

1. Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create two new directories needed for you build.
```
	mkdir build target
	cd build
```
2. Then configure LMMS with CMake, using the previously created target directory.
```
	cmake .. -DCMAKE_INSTALL_PREFIX=../target
```
3. Now compile LMMS: (People with more than one CPU core can use make's -j2 option to compile some files in parallel instead, accelerating the process. Otherwise `make` is just fine. )
```
	make -j2
```

4. Finally you can install LMMS (Optional):
```
	make install
```
5. and, of course, run it:

	../target/bin/lmms