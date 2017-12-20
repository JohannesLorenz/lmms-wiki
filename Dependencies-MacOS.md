## Xcode

* [`Xcode>=4.6.3`](http://stackoverflow.com/a/10335943/3196753)* is required for compilation.
   > \* If recently upgraded OS make sure your XCode isn't outdated by running `open -a XCode`, or via [Applications](https://cloud.githubusercontent.com/assets/6345473/13099744/670d5dfa-d503-11e5-85c3-ad2c99e55c2d.png).

* Xcode Command Line Utilities* must be activated.
   ```bash
   sudo xcode-select --install
   sudo xcodebuild -license
   ```
   > \*Alternately via Applications, Xcode, Xcode Preferences menu, Downloads tab, Command Line Tools, Install.  Or if that's missing, hunt down the installer [from Apple Downloads](https://developer.apple.com/download/more/?=Command%20Line%20Tools%20%28OS%20X%20Mountain%20Lion%29).

## Homebrew
* [Homebrew](https://brew.sh/)* or an equivelent package manager is required for fetching [libraries](Compiling#libraries)
   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
   > \*Alternately, you may use [MacPorts](https://macports.org/) instead.

## Qt5
   ```bash
   brew install git cmake pkgconfig fftw libogg libvorbis lame libsndfile libsamplerate jack \
   sdl libgig libsoundio stk portaudio node fltk qt5

   brew install --build-from-source https://gist.githubusercontent.com/tresf/c9260c43270abd4ce66ff40359588435/raw/fluid-synth.rb
   ```

   > **Note:** If compiling on 10.8, the `stk.rb` and `libgig.rb` brew formula needs a patch and the older `cmake` and `node` are required
   > ```bash
   > brew reinstall --build-from-source https://gist.githubusercontent.com/tresf/efa2cf88156c1f14c1b39c315f1f3ec0/raw/stk.rb
   > brew reinstall --build-from-source https://gist.githubusercontent.com/tresf/efb74f1ec9b600c8aa4e823cc855bef2/raw/libgig.rb
   > brew reinstall --build-from-source https://github.com/tresf/homebrew-core/raw/8aeeb4b5929aa91307f904fae3d42b4fc54387d0/Formula/cmake.rb
   > brew reinstall --build-from-source https://raw.githubusercontent.com/tresf/homebrew-core/2f6bd4138f9af6b26b5bcb066f944c9491fb106d/Formula/node.rb
   > brew intstall qt@5.5
   > ```

## Qt4
   ```bash
   brew install git cmake pkgconfig fftw libogg libvorbis lame libsndfile libsamplerate jack \
   sdl libgig libsoundio stk portaudio node fltk cartr/qt4/qt

   brew install --build-from-source https://gist.githubusercontent.com/tresf/c9260c43270abd4ce66ff40359588435/raw/fluid-synth.rb
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

