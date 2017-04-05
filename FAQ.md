## Project Compression
* The compression format used to compress projects is `qCompress` (`libz`).
* For decompressing projects using lmms, use `lmms -d inputfile.mmpz > output.mmp`
* To decompress a project using a 3rd party language, such as php, see [here](https://gist.github.com/tresf/e5d4f1edb5a93b36f779359eb44b008a).

## Artwork Corrupted
* If LMMS was installed successfully but the artwork displays artifacts, you may have an older LMMS theme selected in the settings. Themes cannot be used between releases.  A `1.0.x` theme will not work with `1.1.x`, etc.
* You may reset the LMMS theme by renaming `~/.lmmsrc.xml` or setting the theme to blank using Preferences, Directories, Theme Directory.  See also issue [#1187](../issues/1187).