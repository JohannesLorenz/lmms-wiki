So, you finally decided to contribute a plugin to LMMS! That's cool! First of all, you should read the sourcecode of one of the provided plugins. Preferably the one that resembles your planned plugin at most. Nevertheless, you will find information on the plugin structure here.

### Required skills ###
* C++/C
* Some experiences with Trolltech's Qt
* Basic DSP knowledge

### Plugin meta data ###
Your plugin has to define a plugin meta data structure in the following style:
```
	extern "C"
	{

	Plugin::Descriptor PLUGIN_EXPORT YourPluginNameHere_plugin_descriptor =
	{
	       STRINGIFY( PLUGIN_NAME ),
	       "YourPluginNameHere",
	       QT_TRANSLATE_NOOP( "pluginBrowser",
		                       "YourPluginDescriptionHere" ),
	       "YourNameHere <YourEmailAdressHere>",
	       0x0100,
	       Plugin::Instrument,
               new PluginPixmapLoader( "logo" ),
               NULL,
               NULL
	} ;

	}
```
Note: This should be in the source (.cpp) file of your plugin.

### Plugin callback ###
LMMS can find your plugin only if you have the following C-style code in your class file. This code is called by LMMS everytime a new instance of the plugin is created.
```
	extern "C"
	{

	// neccessary for getting instance out of shared lib
	Plugin * PLUGIN_EXPORT lmms_plugin_main( Model *, void * _data )
	{
		return( new yourInstrumentClassNameHere( static_cast<InstrumentTrack *>( _data ) ) )
	}

	}
```
Note: This is in the source (.cpp) file for your plugin

### Makefiles ###
* Copy CMakeLists.txt from another plugin source directory and edit it to fit your plugin
* Edit CMakelists.txt in the plugins directory, add a ``` ADD_SUBDIRECTORY(your_plugin_dir) ``` line
* Run cmake, make, make install (anything else? make clean first?)

### Artwork ###
Create two PNG files. One called "logo.png" (48x48 px) which will be displayed as plugin logo in the plugin browser. The other is "artwork.png" (250x250 px) which will be the background/wallpaper for your plugin.

### Hints ###
* Do _not_ give the plugin and the class the same name as this causes conflicts of your classname and your plugin namespace - of course you can use compiler's case-sensitivity, e.g. PLUGIN_NAME in Makefile.am is "myplugin" and your class is called "myPlugin"