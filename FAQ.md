**What is the compression format used to compress projects ?**  
qCompress/qUncompress which itself uses libz. For decompressing projects, use
`lmms -d inputfile.mmpz > output.mmp`

**What language is LMMS programmed in and how can I help contribute to this project?**  
It is programmed with C++ and Qt4 and you can visit the [[LMMS Roadmap|Roadmap]] for details. You can also help by contributing translations, contributing to the Wiki, and more...

**I installed the latest version of LMMS, but the interface is broken! Why?**  
You most likely have an older version of LMMS installed in parallel, and thus still have the older LMMS theme selected in the settings. Older themes (from 0.4.x and older) no longer work in LMMS 1.0.0 and newer. You need to set LMMS to use the new default theme.