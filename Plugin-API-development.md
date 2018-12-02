# Plugin API development

## Basics

Plugin APIs are, as the name says, generic interfaces that support multiple plugins. Examples are VST, LADSPA, or LV2 (in work).

In LMMS, this is realized by having a proxy plugin, which does not directly do computation, but gets a `Plugin::Descriptor::SubPluginFeatures*` passed. The reference contains data to the real plugin; and the proxy plugin can use the reference to do computations. For example, a call to the overridden `Effect::processAudioBuffer` in the proxy plugin could do some preparations and then call some processing function looked up with the `SubPluginFeatures`.

The `SubPluginFeatures` are generated when selecting a plugin, e.g. in the `EffectSelectDialog`: If the plugin's `Descriptor` has `SubPluginFeatures`, keys are fetched from `SubPluginFeatures::listSubPluginKeys`, otherwise, just one plugin key is created for the whole plugin. In `SubPluginFeatures::listSubPluginKeys`, the plugin can add plugin specific attributes to the keys, like file or plugin names. When an Effect has been chosen, `Plugin::instantiate` finally gets the right key passed.

## File structure

Starting with the plugin itself, the layout is [as usual](Plugin-development.md):

* For instruments, one file with classes inheriting `Instrument` and `InstrumentView` (see, e.g., the ZynAddSubFx plugin)
* For effects, use three files inheriting `Effect`, `EffectControls` and `EffectControlDialog` (see, e.g., the Amplifier plugin)
* Everything shared (e.g. common files between an LV2 instrument and an LV2 effect) should go into include/plugin-common and src/plugin-common (proposal, currently it's in include and src/core)

Now, inherit from `SubPluginFeatures` (see the LadspaEffect plugin) and override the virtual functions.

As the `SubPluginFeatures` are generic, they don't allow to store plugin descriptors or similar. The `SubPluginFeatures` contain the plugin library names, so you could load them when the `SubPluginFeatures` require it. In order to be more efficient, LADSPA introduced a `LadspaManager`, which stores all the descriptors and additional info, and which can be (roughly) looked up by `SubPluginFeatures::Key`. For example, this lookup is being done in the `LadspaEffect` constructor.
