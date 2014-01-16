**What is the compression format used to compress projects ?**  
qCompress/qUncompress which itself uses libz. For decompressing projects, use
lmms -d inputfile.mmpz > output.mmp

**What language is LMMS programmed in and how can I help contribute to this project?**  
It is programmed with C++ and Qt4 and you can visit the [[LMMS Roadmap|Roadmap]] for details. You can also help by contributing translations, contributing to the Wiki, and more...

*Note: Is this even development-related?*  
**What is the fastest way to change the frequencies of many notes at once? I plan to make the higher notes swap places with the lower notes.**  
A fast and easy way to, not only manipulate the frequencies of many notes at once, but many buttons and knobs can also be manipulated as well using the Automation tracks. Automation is a beautiful thing in that it 'automatically' adjusts to the settings that you created. Another way of thinking about Automation, let's say volume manipulation, is that it can automatically fade in from adjusting the dB levels from 0 to 7 (most commonly). So after you set the Automation track to adjust the volume levels from 0dB to 7dB, when you play back from the playlist, it will automatically fade in the volume for the Volume Controller (or volume knob) as the song progresses during play-back. Refer to the Documentation for more information.