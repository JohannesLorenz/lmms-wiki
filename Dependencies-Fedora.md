# Qt4
On Fedora 25:
```
dnf install qt4-devel libsndfile-devel fftw3-devel libvorbis-devel \
    libsamplerate-devel libogg-devel stk-devel fltk-devel doxygen \
    lib-alsa-devel pulseaudio-libs-devel zynaddsubfx
```

# Qt5
```bash
sudo dnf install qt5-devel libsndfile-devel fftw3-devel libvorbis-devel \
     libsamplerate-devel libogg-devel stk-devel fltk-devel fltk-fluid \
     fluidsynth-devel alsa-lib-devel pulseaudio-libs-devel gcc-c++ xcb-util-devel \
     xcb-util-keysyms-devel \
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting
Note for Fedora users: For 32-bit VST support `wine.i686` and `wine-devel.i686` are required.