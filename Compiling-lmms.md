### Dependencies

The following dependencies should be met before compiling LMMS:

Required libraries:

* Qt >= 4.3.0 with devel-files (4.4.x recommended)

Optional, but strongly recommended (with devel-files each):
* JACK
* libvorbis & libogg
* libalsa
* SDL
* libsamplerate (>= 0.1.7)
* libsndfile
* WINE
* stk, libstk
* libfluidsynth
* fftw3

For building, you'll also need:
* GCC with g++
* CMake (>= 2.4.5 required, >=2.6.0 recommended)

### Building on Linux

Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create two new directories needed for you build.

	mkdir build target
	cd build

Then configure LMMS with CMake, using the previously created target directory.

	cmake .. -DCMAKE_INSTALL_PREFIX=../target

Now compile LMMS: (People with more than one CPU core can use make's -j2 option to compile some files in parallel instead, accelerating the process. Otherwise `make` is just fine. )

	make -j2

Finally you can install LMMS:

	make install

and, of course, run it:

	../target/bin/lmms