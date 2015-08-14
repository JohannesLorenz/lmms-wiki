### Dependencies

**The following dependencies should be met before compiling LMMS:**

* [Apple OS X (10.7 "Lion" or higher)](https://itunes.apple.com/app/id675248567) (5.3GB)
* [Xcode 4.6.3 or higher](https://itunes.apple.com/app/id497799835) (2.2GB)
* Xcode Command Line Utilities
   ```sh
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

1. Install brew dependencies using travis install file

   ```bash
   sh lmms/.travis/osx..install.sh
   ```

1.  Note if you receive the following curl SSL error, this is most likely due to a missing SSL certificate.  To fetch the latest certificates, run apple software updates, reboot and try again.

   ```
   curl: (35) error:14077458:SSL routines:SSL23_GET_SERVER_HELLO:reason(1112)
   Failed to download resource ".."
   Download failed ... 
   ```

### MacPorts
*Skip this section if using Homebrew for fetching dependencies*

1. Install MacPorts per https://www.macports.org/install.php
1. Update the collection of available port definitions:

   ```sh
   sudo port selfupdate
   ```

1. 
  1. Install LMMS and all of its dependencies automatically:

    ```sh
    sudo port install lmms
    ```

    The LMMS application bundle will be in your MacPorts applications folder, which is /Applications/MacPorts unless you changed it.

  1. Alternately, if you would like to use MacPorts only to get the dependencies and compile LMMS from source manually:

    ```sh
    sudo port install cmake fftw-3-single fltk \
    fluidsynth jack libgig libogg libsamplerate libsdl \
    libsndfile libvorbis portaudio qt4-mac stk pkgconfig
    ```

### Compiling
*See also [Compiling lmms (Linux)](Compiling-lmms)*

**Instructions on compiling and installing LMMS:**

1. Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create a new directory which is needed for you build.

    ```sh
    cd
    cd lmms
    mkdir build
    ```
1. Optionally, you can also create a "target" directory, or you can install LMMS to any *empty* directory of your choice.

    ```sh
    mkdir target
    ```
1. Then configure LMMS with CMake, using the previously created target directory (or any *empty* directory of your choice, in which case just replace "../target" with the directory you want to use).

    ```sh
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=../target
    ```

    > **Note:** If you receive the error `/usr/bin/xcode-select` returned unexpected error, make sure to install XCode as well as the XCode Command line tools for your platform and retry.

    > **Note:** To build for older versions (i.e. OS X 10.8), you will need the corresponding Xcode SDK installed.
    > * Specify the target using: `-DCMAKE_OSX_DEPLOYMENT_TARGET=10.8`

1. Now compile LMMS:
    > **Note:** `make -j2`, commonly used on Linux, won't work here.

    ```sh
    make
    ```

1. Extract a copy of the stk source code in your home directory (`make install` will copy the rawwaves directory from the stk source directory into the LMMS application bundle):

    ```sh
    cd
    curl -O https://ccrma.stanford.edu/software/stk/release/stk-4.5.0.tar.gz
    tar xopzf stk-4.5.0.tar.gz
    ```

1. Finally, install LMMS into the previously specified directory:

    ```sh
    make install
    ```

1. This will automatically create an application bundle on the Desktop.
   ![image](https://cloud.githubusercontent.com/assets/6345473/2878829/dfc7c7ca-d461-11e3-991d-163e9b7e91ae.png)

   ![image](https://cloud.githubusercontent.com/assets/6345473/2587591/79b3ea50-ba25-11e3-8513-a61085528a6d.png)
   <br>*You may copy this to Applications or run it directly from the Desktop*

1. Optionally, you may wish to package this into a DMG (FIXME Lion has issues with DMG artwork, etc):
    
    ```sh
    ~/Desktop/create_apple_dmg.sh
    ```

    ![image](https://cloud.githubusercontent.com/assets/6345473/2587649/06a5d634-ba27-11e3-941b-0dab79ff3f8f.png)

### Debugging LMMS OSX
 1. Clean the build environment

   ```sh
   make clean
   rm -rf CMakeCache.txt
   ```
 1. Re-run cmake configure step with the following additional parameters:

   ```sh
   -DCMAKE_BUILD_TYPE=Debug -DWANT_SWH=OFF
   ```
 1. Re-run the `make install` step
 1. Launch LMMS with the lldb debugger.  You will be prompted for your password.

    ```sh
    lldb ~/Desktop/LMMS.app/Contents/MacOS/lmms
    ```
 1. Reproduce the crash.  Hit CTRL+C.  Type this command from lldb for a backtrace:

    ```sh
    thread backtrace all
    ```
 1. Post the back-trace to https://gist.github.com/ and paste the link to a new bug report.