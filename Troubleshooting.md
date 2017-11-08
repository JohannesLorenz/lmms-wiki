# Troubleshooting
[Windows](#windows) | [Linux](#linux) | [MacOS](#macos)

Troubleshooting guide for running LMMS on various platforms.  You may also ask in our `#support` channel on [Discord](https://lmms.io/chat).  For development questions, please see our [compiling](compiling) page(s) instead.

## Windows

### Clean Install

 * Reboot your computer (ensures no instances of `lmms.exe` are already running)
 * Uninstall LMMS from Control Panel
 * Locate `C:\Program Files\LMMS\` if present, remove it (if paranoid, move it to your desktop).
 * If you have a `C:\Program Files (x86)\LMMS`, remove it (again, if paranoid, move it instead).
 * Locate `%USERPROFILE%\.lmmsrc.xml`, remove it (again, if paranoid, move it instead)
 * Reinstall LMMS
 
If LMMS launches, great.  If it doesn't, search for Qt conflicts:

### Qt Conflicts
 * Search your computer for any files named `Qt*.dll` (Windows + F, Qt*.dll, wait for it to finish).
   * Take special note of any DLLs that may be in your `PATH`, such as `C:\Windows`, `C:\Windows\System32`, etc.  You may need to delete them manually or uninstall the application associated with them.
   * Ignore those found in `%APPDATA%` or `Program Files`, they're generally safe.  Watch out for those installed in `C:\Windows` or `C:\Windows\System32`

### Sound Driver Conflicts
 * This is the most common cause of crashes on startup.
 * First, download the latest sound driver from your sound card (from PC manufacturer website, or directly from the sound card manufacturer.)
   * **Note:** Some HDMI sound devices use video drivers from NVIDIA/AMD(ATI)/Intel.
 * If this doesn't help, force removal of the 3rd party driver and use the one provided by Microsoft.  This is how:
   1.  Open Device Manager
   2.  Expand Sound and Game controllers 
   3.  Locate your sound card, Right Click, Remove Device
   4.  Click the Checkbox that says "Remove driver software for this device"
   5.  Reboot
   6.  Windows should automatically search for a sound driver.  This will take a few minutes.
   7.  Windows should install a sound driver from Windows updates.  Once finished, reboot if needed.
   8.  If Windows could not located a sound driver, visit the manufacturer website and manually download a sound driver for your computer, install the sound driver manually, reboot if needed.
   9.  Try LMMS again. 
* If this doesn't help, try replacing the [`libportaudio-2.dll`  in your installation directory with a recompiled one](https://github.com/LMMS/lmms/issues/451#issuecomment-37773385).

If that doesn't work, you may be able to [shim a 3rd-party DLL into the software](https://github.com/LMMS/lmms/issues/1865#issuecomment-79510559).  If not, you're out of luck.

## Linux
Sorry, there's nothing here yet!  Please help by [contributing](https://lmms.io/get-involved/) or ask in our `#support` channel on [Discord](https://lmms.io/chat).


## MacOS
Sorry, there's nothing here yet!  Please help by [contributing](https://lmms.io/get-involved/) or ask in our `#support` channel on [Discord](https://lmms.io/chat).

