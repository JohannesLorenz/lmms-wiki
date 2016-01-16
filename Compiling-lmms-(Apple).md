### Dependencies

**The following dependencies should be met before compiling LMMS:**

* [Apple OS X (10.7 "Lion" or higher)](https://itunes.apple.com/app/id675248567) (5.3GB)
* [Xcode 4.6.3 or higher](https://itunes.apple.com/app/id497799835) (2.2GB)
* Xcode Command Line Utilities
   ```bash
   sudo xcode-select --install
   sudo xcodebuild -license
   ```
   > **Note:** Alternately, you may install the command line tools via **Applications, Xcode, Xcode Preferences menu, Downloads tab, Command Line Tools, Install**

* [Homebrew](#homebrew) or [MacPorts](#macports) required for fetching build dependencies


### Homebrew
*Skip this section if using MacPorts for fetching dependencies*

1. Install Homebrew per http://brew.sh
1. Update brew

   ```bash
   brew update
   ```
1. Install git

   ```bash
   brew install git
   ```

1. Clone source code

   ```bash
   git clone https://github.com/lmms/lmms
   ```

   > **Note:** Optional, to clone [a specific branch](https://github.com/lmms/lmms/branches) use the -b switch e.g. `git clone -b master https://github.com/lmms/lmms`

1. Install brew dependencies using travis install file

   ```bash
   sh lmms/.travis/osx..install.sh
   ```

1. Install appdmg (Needed only for for packaging the DMG file)

   ```bash
   brew install node
   npm install -g appdmg
   ```

1.  Note if you receive the following curl SSL error, this is most likely due to a missing SSL certificate.  To fetch the latest certificates, run apple software updates, reboot and try again.

   ```
   curl: (35) error:14077458:SSL routines:SSL23_GET_SERVER_HELLO:reason(1112)
   Failed to download resource ".."
   Download failed ... 
   ```

1. Continue on to the [#compiling](Compiling) section

### MacPorts
*Skip this section if using Homebrew for fetching dependencies*

1. Install MacPorts per https://www.macports.org/install.php
1. Update the collection of available port definitions:

   ```bash
   sudo port selfupdate
   ```

1. 
  1. Install LMMS and all of its dependencies automatically:

    ```bash
    sudo port install lmms
    ```

    The LMMS application bundle will be in your MacPorts applications folder, which is /Applications/MacPorts unless you changed it.

  1. Alternately, if you would like to use MacPorts only to get the dependencies and compile LMMS from source manually:

    ```bash
    sudo port install cmake fftw-3-single fltk \
    fluidsynth jack libgig libogg libsamplerate libsdl \
    libsndfile libvorbis portaudio qt4-mac stk pkgconfig \
    node npm
    ```

  1. Install appdmg (Needed only for for packaging the DMG file)

    ```bash
    sudo npm install -g appdmg
    ```

1. Continue on to the [#compiling](Compiling) section

### Compiling
*See also [Compiling lmms (Linux)](Compiling-lmms)*

1. Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create a new directory which is needed for you build.

    ```bash
    cd
    cd lmms
    mkdir build
    ```
1. Optionally, you can also create a "target" directory, or you can install LMMS to any *empty* directory of your choice.

    ```bash
    mkdir target
    ```
1. Then configure LMMS with CMake, using the previously created target directory (or any *empty* directory of your choice, in which case just replace "../target" with the directory you want to use).

    ```bash
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=../target
    ```

    > **Note:** If you receive the error `/usr/bin/xcode-select` returned unexpected error
    >  * Make sure to install XCode as well as the XCode Command line tools for your platform
    >  * Remove CMakeCache.txt
    >  * Retry

    > **Note:** To build for older versions (i.e. OS X 10.8), you will need the corresponding Xcode SDK installed.
    > * Specify the target using: `-DCMAKE_OSX_DEPLOYMENT_TARGET=10.8`

1. Now compile LMMS:
    > **Note:** `make -j2`, commonly used on Linux, won't work here.

    ```bash
    make
    ```

1. Run LMMS:

    ```bash
    ./lmms
    ```

### Installing

1. You only need to do this step once.  Extract a copy of the stk source code in your home directory (`make install` will copy the rawwaves directory from the stk source directory into the `LMMS.app` application bundle):

    ```bash
    cd
    curl -O https://ccrma.stanford.edu/software/stk/release/stk-4.5.0.tar.gz
    tar xopzf stk-4.5.0.tar.gz
    ```

1. Finally, install LMMS into the previously specified directory:

    ```bash
    make install
    ```

1. This will automatically create an `LMMS.app` application bundle in the `build` directory.
   ![image](https://cloud.githubusercontent.com/assets/6345473/9310513/1abf0576-44de-11e5-8589-d33d6c1c5b73.png)

   ![image](https://cloud.githubusercontent.com/assets/6345473/9310604/c6097a74-44de-11e5-9d93-43f950622241.png)
   <br>*You may copy this to `Applications` or run it directly from the `build` directory*

### Packaging

1. Optionally, you may wish to package this into a DMG:
    
    ```bash
    make dmg
    ```

    ![image](https://cloud.githubusercontent.com/assets/6345473/9310558/6e5e11d6-44de-11e5-8f56-233387f3f3e5.png)

### Debugging LMMS OSX
 1. Clean the build environment

   ```bash
   make clean
   rm -rf CMakeCache.txt
   ```
 1. Re-run cmake configure step with the following additional parameters:

   ```bash
   -DCMAKE_BUILD_TYPE=Debug -DWANT_SWH=OFF
   ```
 1. Re-run the `make install` step
 1. Launch LMMS with the lldb debugger.  You will be prompted for your password.

    ```bash
    lldb ~/Desktop/LMMS.app/Contents/MacOS/lmms
    ```
 1. Reproduce the crash.  Hit CTRL+C.  Type this command from lldb for a backtrace:

    ```bash
    thread backtrace all
    ```
 1. Post the back-trace to https://gist.github.com/ and paste the link to a new bug report.