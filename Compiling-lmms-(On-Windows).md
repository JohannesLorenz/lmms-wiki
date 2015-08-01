# Compiling LMMS on Windows
### WARNING:  This is an experimental process.  Proceed at your own risk.

 * **Note:** This tutorial was created in an effort to enable code debugging on Windows.  This tutorial is not yet supported.  Please find our official Windows build tutorial here: [Compiling-lmms-(Windows)](https://github.com/LMMS/lmms/wiki/Compiling-lmms-(Windows))
 * **Note:** The pacman mirrors that come default with msys2 rely heavily on sourceforge.net mirrors.  If you are having problems with pacman, first make sure sourceforge isn't experiencing downtime.

###Building LMMS Using MSYS2 and mingw-w64 on Windows 64-bit

 1. Download and install 64-bit `msys2` from https://msys2.github.io/

 1. Launch MSYS2 Shell, update (about 16MB):
   
   Sync your local database:
   ```bash
   pacman -Sy
   ```
   Update essential packages:
   ```bash
   pacman --needed -S bash pacman pacman-mirrors msys2-runtime
   ```
   Restart MSYS2 (important), and update the rest of your packages:
   ```bash
   pacman -Su
   ```
  > **Note:** If at any time you receive the message `Errors occurred, no packages were upgraded`, try again.

 1. Download and install the 32-bit and 64-bit toolchains (about 85MB)

   ```bash
   pacman -S mingw-w64-x86_64-gcc mingw-w64-i686-gcc
   ```
 1. Download and install dependencies (about 726MB, 3.3GB installed)

   ```bash
   pacman -S git pkgconfig make cmake wget p7zip gzip tar binutils mingw-w64-x86_64-qt4 mingw-w64-i686-qt4 gdb
   ```

 1. Close and re-open msys2.  Download `msys2_helper.sh` helper script

   ```bash
   #FIXME: Change this URL to https://github.com/lmms/[...]
   wget https://raw.githubusercontent.com/tresf/lmms/master/data/scripts/msys2_helper.sh --no-check-certificate
   ```

 1. Run the helper script.  This will automatically:
   * Download, extract and install the mingw ppa (400MB)
   * Download, compile and install `fluid.exe`
   * Configure `git` for use with msys

   ```bash
   ./msys2_helper.sh
   ```
   > **Note:** You will eventually receive some messages `cp: cannot create regular file`, these are safe to ignore.

   > **Note2:** Fluid may show warnings during build, these are generally safe to ignore as well.

 1. Create symbolic links for CMD-style paths (workaround msys2's [`moc.exe`](https://gist.github.com/tresf/de0aad39c36e076e61a1) issue)

   * Open CMD as administrator (Start, "CMD", `CTRL + SHIFT + ENTER`)
   * Run the following commands:

      ```cmd
      mklink /d %SystemDrive%\mingw64 %SystemDrive%\msys64\mingw64
      mklink /d %SystemDrive%\mingw32 %SystemDrive%\msys64\mingw32
      mklink /d %SystemDrive%\home %SystemDrive%\msys64\home
      ```

###Compiling

 1. Run configure

  ```bash
  cd ~/lmms
  mkdir build target
  cd build
  ../build_mingw64
  ```

 1. Build

  ```bash
  make VERBOSE=1
  ```

###Running

 1. Run
  ```bash
  ./lmms.exe
  ```

 1. (TODO) Fix artwork directory.  Installing and running the Windows desktop version will help resolve artwork.

###Debugging

 1. Enable debug symbols to be passed to mingw script

    ```bash
    export CMAKE_OPTS=-DCMAKE_BUILD_TYPE=Debug
    ```

 1. Add manual reference to `QtCore4.dll`
    > **Note:** FIXME:  Can we add this automatically via `IF(CMAKE_BUILD_TYPE STREQUAL "Debug")`

    * `src/CMakeLists.txt:115`

      ```cmake
      TARGET_LINK_LIBRARIES(lmms
          ${LMMS_REQUIRED_LIBS} QtCore4
          # Fix debug builds ---^
      )
      ```

    * `plugins/zynaddsubfx/CMakeLists.txt:112`

      ```cmake
      TARGET_LINK_LIBRARIES(ZynAddSubFxCore zynaddsubfx_nio ${FFTW3F_LIBRARIES} 
      ${QT_LIBRARIES} -lz -lpthread QtCore4)
      #         Fix debug builds ---^
      ```

 1. Remove the build directory and run the appropriate build script again
 1. To debug the lmms.exe process

    ```bash
    gdb lmms.exe
    run
    ```

###Packaging

 1. Install NSIS from http://nsis.sourceforge.net/Download

 1. Add NSIS to Windows PATH
     * Navigate to View Advanced System Settings, Environment Variables
     * Add this to the very end of the System PATH

    ```bash
    ;c:\Program Files (x86)\nsis\
    ```

 1. Create the package

    ```bash
    make package
    ```
