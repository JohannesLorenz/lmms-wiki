# Compiling LMMS on Windows
### Building LMMS Using MSYS2 and mingw-w64 on Windows 64-bit

 * LMMS compiled and running on Windows 7 64-bit

   ![image](https://cloud.githubusercontent.com/assets/6345473/9337465/82d76fe8-45ad-11e5-9b8b-5fdabb87d32b.png)

 * **Note:** This tutorial was created in an effort to enable code debugging on Windows.  This tutorial is not yet supported.  Please find our official Windows build tutorial here: [Compiling-lmms-(Windows)](https://github.com/LMMS/lmms/wiki/Compiling-lmms-(Windows))
 * **Note:** The pacman mirrors that come default with msys2 rely heavily on sourceforge.net mirrors.  If you are having problems with pacman, first make sure sourceforge isn't experiencing downtime.

 * **Note:** It's a good idea to avoid spaces in your directories, including your windows username!

###Install Dependencies

 1. Download and install 64-bit `msys2` from https://msys2.github.io/

> **Note:** It's easiest to install `msys2` as a direct sub folder of your main drive.

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

 1. Close msys2.

 1. Open Mingw-w64

 1. Download `msys_helper.sh` helper script

   ```bash
   wget https://raw.githubusercontent.com/lmms/lmms/master/cmake/msys/msys_helper.sh --no-check-certificate
   ```

 1. Run the helper script.  This will automatically:
   * Download, extract and install the mingw ppa (400MB)
   * Download, compile and install `fluid.exe`
   * Configure `git` for use with msys

   ```bash
   ./msys_helper.sh
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

   * **Note**:  If you installed msys2 to a non-standard location, these commands will need to be adjusted to reflect your install location.  `%SystemDrive%` generally is `C:\`, so you can adjust to `D:\`, `E:\` as needed.

###Compiling

 1. Run configure

  ```bash
  cd ~/lmms
  mkdir build target
  cd build
  ../cmake/build_mingw64.sh
  ```

 1. Build

  ```bash
  make VERBOSE=1
  ```

  > **Note:** win32 builds need fluid to be rebuilt by the 32-bit compiler.  The easiest way to do this is open MinGW-w64 **Win32**-Shell:

  ```bash
  rm -rf /usr/local/bin/fluid.exe
  . ~/msys_helper.sh
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