## Xcode

* [`Xcode>=4.6.3`](http://stackoverflow.com/a/10335943/3196753)* is required for compilation.
   > \* If recently upgraded OS make sure your XCode isn't outdated by running `open -a XCode`, or via [Applications](https://cloud.githubusercontent.com/assets/6345473/13099744/670d5dfa-d503-11e5-85c3-ad2c99e55c2d.png).

* Xcode Command Line Utilities* must be activated.
   ```bash
   sudo xcode-select --install
   sudo xcodebuild -license
   ```
   > \*Alternately via Applications, Xcode, Xcode Preferences menu, Downloads tab, Command Line Tools, Install

## Homebrew
* [Homebrew](https://brew.sh/)* or an equivelent package manager is required for fetching [libraries](Compiling#libraries)
   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
   > \*Alternately, you may use [MacPorts](https://macports.org/) instead.

## Qt5
   ```bash
   brew install git cmake pkgconfig fftw libogg libvorbis libsndfile libsamplerate jack \
   sdl libgig libsoundio stk portaudio node fltk qt@5.5

   brew install --build-from-source https://github.com/LMMS/lmms/raw/master/cmake/apple/fluid-synth.rb
   ```

## Qt4
   ```bash
   brew install git cmake pkgconfig fftw libogg libvorbis libsndfile libsamplerate jack \
   sdl libgig libsoundio stk portaudio node fltk cartr/qt4/qt

   brew install --build-from-source https://github.com/LMMS/lmms/raw/master/cmake/apple/fluid-synth.rb
   ```

&nbsp;&nbsp;&nbsp;&nbsp;...done installing?  Next, [clone the source code](Compiling#clone-source-code)
<br><!-- End Section--><br>

## Troubleshooting

### Library Loading Issues
   ```bash
   export DYLD_PRINT_LIBRARIES=1
   /Applications/LMMS.app/Contents/MacOS/lmms  # or build/LMMS.app/Contents/MacOS/lmms
   ```

&nbsp;&nbsp;&nbsp;...want to [add something](dependencies-opensuse/_edit)?

