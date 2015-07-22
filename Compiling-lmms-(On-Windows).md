# Compiling LMMS on Windows
## WARNING:  This is an experimental process.  Proceed at your own risk.

 * **Note:** This tutorial was created in an effort to enable code debugging on Windows.  This tutorial is not yet supported.  Please find our official Windows build tutorial here: [Compiling-lmms-(On-Windows)](https://github.com/LMMS/lmms/wiki/Compiling-lmms-(On-Windows))
 * **Note:** The pacman mirrors that come default with msys2 rely heavily on sourceforge.net mirrors.  If you are having problems with pacman, first make sure sourceforge isn't experiencing downtime.

###Building LMMS Using MSYS2 and mingw-w64 on Windows 64-bit

 1. Download and install `msys2` from https://msys2.github.io/

 1. Launch MSYS2 shell, update:

   ```bash
   pacman -Syu
   ```

 1. Install the 32-bit and 64-bit toolchains

   ```bash
   pacman -S mingw-w64-x86_64-gcc
   pacman -S mingw-w64-i686-gcc
   ```
 1. Install dependencies (warning, over 600MB download, 3GB installed)

   ```bash
   pacman -S git pkgconfig make wget p7zip gzip tar binutils mingw-w64-x86_64-qt4 mingw-w64-i686-qt4
   ```

 1. Download `fetch_ppa.sh` helper script

   ```bash
   wget https://gist.githubusercontent.com/Umcaruje/1966a7a747fb393c41e6/raw/de80e4010b7d66525604a4da48750ea10af37ee2/fetch_ppa.sh --no-check-certificate
   ```

 1. Download `extract_debs.sh` helper script

   ```bash
   wget https://gist.githubusercontent.com/Umcaruje/1966a7a747fb393c41e6/raw/46c88c892c3db8d1f7b0491def3d2d6413818be6/extract_debs.sh --no-check-certificate
   ```

 1. Download deb packages (takes about 10 minutes on a good connection)

   ```bash
   ./fetch_ppa.sh
   ```

 1. Extract DLLs from deb packages

   ```bash
   ./extract_debs.sh
   ```

 1. Change permissions of `qconfig.h` to allow overwrite:
   ```bash
   chmod u+w /mingw64/include/qt4/Qt/qconfig.h
   chmod u+w /mingw64/include/qt4/QtCore/qconfig.h
   ```
 1. Copy the `mingw32` and `mingw64` directories to `/` (to match msys2 environment)

   ```bash
   # \cp = clobber existing files, mandatory!
   \cp -r ppa/opt/mingw* /
   ```

 1. Ignore `~/ppa/usr` (only contains command documentation)

 1. Configure git to ignore bad certs (necessary evil unless you want to setup CA certs)

   ```bash
   mkdir .git; touch .git/config
   git config --global http.sslverify false
   ```
 
 1. Clone the code repository

  ```bash
  git clone -b master https://github.com/lmms/lmms.git
  ``` 

 1. Patch Win64 toolchain file `cmake/modules/Win64Toolchain.cmake`
  ```cmake
  SET(MINGW_PREFIX /mingw64)
  SET(MINGW_PREFIX32 /mingw32)
  ```

 1. Patch MinGW toolchain file `cmake/modules/MinGWCrossCompile.cmake`
 
  ```cmake
  # line :10, comment out:
  # SET(MINGW_TOOL_PREFIX ...

  # line :28, comment out:
  # SET(PKG_CONFIG_EXECUTABLE ...

  # line :31, comment out:
  # SET(QT_QMAKE_EXECUTABLE ...
  ```

 1. Disable using C/C++ response files `CMakeLists.txt`

  ```cmake
  # line :6
  SET(CMAKE_C_USE_RESPONSE_FILE_FOR_INCLUDES OFF)
  SET(CMAKE_C_USE_RESPONSE_FILE_FOR_OBJECTS OFF)
  SET(CMAKE_CXX_USE_RESPONSE_FILE_FOR_INCLUDES OFF)
  SET(CMAKE_CXX_RESPONSE_FILE_FOR_OBJECTS OFF)
  ```

 1. Create symbolic links for CMD-style paths (workaround msys2's [`moc.exe`](https://gist.github.com/tresf/de0aad39c36e076e61a1) issue)

   * Open CMD as administrator (Start, "CMD", `CTRL + SHIFT + ENTER`)
   * Run the following commands:

      ```cmd
      mklink /d C:\mingw64 c:\msys64\mingw64
      mklink /d C:\home c:\msys64\home
      ```
 1. Build `fluid.exe` from source

   ```bash
   wget http://fltk.org/pub/fltk/1.3.3/fltk-1.3.3-source.tar.gz
   tar zxf fltk-1.3.3-source.tar.gz
   rm fltk-1.3.3-source.tar.gz
   cd fltk-1.3.3
   ./configure
   make
   make install
   cd ~/lmms
   ```


 1. Change path to fluid executable in `plugins/zynaddsubfx/CMakeLists.txt`

   ```cmake
   IF(MINGW_PREFIX)
        SET(FLTK_FLUID_EXECUTABLE "/usr/local/bin/fluid")
   #                    HERE -----^
   ENDIF()
   ```
 1. Enable `-fpermissive` in Zyn's make file `plugins/zynaddsubfx/CMakeLists.txt`

  ```cmake
  # line :20
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -Wno-write-strings -Wno-deprecated-declarations -fpermissive")
  #                                                                          HERE ------------^
  ```

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

 1. Run
  ```bash
  ./lmms.exe
  ```

 1. (TODO) Fix artwork directory.  Installing and running the Windows desktop version will help resolve artwork.