## Qt5

```bash
zypper install libportaudio-devel fftw3-devel glibc-devel-32bit libsndfile-devel \
libstdc++-devel-32bit libstdc++48-devel-32bit libstdc++6-devel-gcc6 \
libstdc++6-devel-gcc6-32bit gcc-32bit gcc-c++-32bit fluidsynth fluidsynth-devel \
libgig-devel wine-devel wine-devel-32bit libQt5Core-devel libqt5-qttools \
libqt5-qttools-devel
```

## Qt4

```bash
zypper install libportaudio-devel fftw3-devel glibc-devel-32bit libsndfile-devel \
libstdc++-devel-32bit libstdc++48-devel-32bit libstdc++6-devel-gcc6 \
libstdc++6-devel-gcc6-32bit gcc-32bit gcc-c++-32bit fluidsynth fluidsynth-devel \
libgig-devel wine-devel wine-devel-32bit libqt4-devel
```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

The following package has been reported to fix `wine gcc++ failed`
```bash
zypper install gcc48-32bit
```