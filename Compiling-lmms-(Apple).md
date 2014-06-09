### Dependencies

**The following dependencies should be met before compiling LMMS:**

* [Apple OS X Mavericks 10.9](https://itunes.apple.com/app/id675248567) (5.3GB)
* [MacPorts](https://www.macports.org/install.php)
* [Xcode 5.1](https://itunes.apple.com/app/id497799835) (2.2GB)
* Xcode Command Line Utilities (Xcode Preferences menu, Downloads tab, Command Line Tools, Install)

**Fetching requirements using MacPorts**
*See also [Compiling lmms (Linux)](Compiling-lmms)*

1. Update the list of available port repositories:

   ```sh
   sudo port selfupdate
   ```

2. Fetch all available dependencies:

    ```sh
    sudo port -v install cmake fftw-3-single fltk \
    fluidsynth jack libogg libsamplerate libsdl \
    libsndfile libvorbis portaudio qt4-mac pkgconfig
    ```

    *Note, VST, OpulenZ are not available at this time.*

3. Fetch and compile STK (required for Mallets)

    ```sh
    cd
    curl https://ccrma.stanford.edu/software/stk/release/stk-4.4.4.tar.gz > ~/stk-4.4.4.tar.gz
    gunzip -c stk-4.4.4.tar.gz | tar xopf -
    cd stk-4.4.4
    sudo cp include/*.h /opt/local/include
    ./configure
    cd src
    make
    sudo cp libstk.a /opt/local/lib
    ```

### Compiling

**Instructions on compiling and installing LMMS:**

1. If you're building a version newer than 1.0, you have to fetch the ZynAddSubFx source first:

    ```sh
    cd lmms
    git submodule update --init --recursive
    ```

1. Assuming you have already fetched the sources (see [[Accessing git repository]] if not), switch to the source root directory, and create a new directory which is needed for you build.

    ```sh
    mkdir build
    ```
2. Optionally, you can also create a "target" directory, or you can install LMMS to any directory of your choice.
    <br>*Warning, If you choose something other than `$HOME/lmms/target` you will later need to modify data/create_apple_installer.sh.*

    ```sh
    mkdir target
    ```
3. Then configure LMMS with CMake, using the previously created target directory (or any directory of your choice, in which case just replace "../target" with the directory you want to use).

    ```sh
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=../target
    ```
4. Now compile LMMS:
    <br>*Warning, "make -j2" -- commonly used on Linux -- won't work here.*

    ```sh
    make
    ```
5. Finally, install LMMS into the previously specified directory (this will bundle it to the desktop as well)

    ```sh
    make install
    ```
6. This will automatically bundle it to the Desktop.
   ![image](https://cloud.githubusercontent.com/assets/6345473/2878829/dfc7c7ca-d461-11e3-991d-163e9b7e91ae.png)

   ![image](https://cloud.githubusercontent.com/assets/6345473/2587591/79b3ea50-ba25-11e3-8513-a61085528a6d.png)
   <br>*You may copy this to Applications or run directly from the Desktop*

8. Optionally, you may wish to package this into a DMG:
    
    ```sh
    ~/Desktop/create_apple_dmg.sh
    ```

    ![image](https://cloud.githubusercontent.com/assets/6345473/2587649/06a5d634-ba27-11e3-941b-0dab79ff3f8f.png)

### Debugging LMMS OSX
 1. Re-run cmake configure step with the following additional parameter:

   ```sh
   -DCMAKE_BUILD_TYPE=Debug
   ```
 1. Re-run the `make install` step
 1. Launch LMMS with the lldb debugger.  You will be prompted for your password.

    ```sh
    lldb ~/Desktop/LMMS.app/Contents/MacOS/lmms
    ```
 1. Reproduce the crash.  Hit CTRL+C.  Type this command from lldb for a backtrace:

    ```sh
    thread back-trace all
    ```
 1. Post the back-trace to https://gist.github.com/ and paste the link to a new bug report.
