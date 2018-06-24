# Introduction

There should be some documentation about LMMS' core architecture and concepts 
to help newcomers with some development skills to understand how it works.

This page is an attempt to aggregate the informations found.

# Model and ModelView

LMMS uses a design pattern similar to [Model–view–controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller). In LMMS, most of views also play a role as controllers.
Model emits `Model::dataChanged` signal when its data is changed and `Model::propertiesChanged` when its property(range, step size, etc.). These signals are used for updating corresponding views(see `ModelView::doConnections`) and tracking changes in models.

`ModelView` holds reference to its model. It *doesn't own the model* except the model is default-constructed. Existing `ModelView` instance can be reused by setting new model using `ModelView::setModel`. Views handle model change by overriding `modelChanged()`.

# BB tracks

A list of TCOs(`TrackContentObjects`) by calling `Track::getTCOs` for beat/bassline(BB) tracks(type `Track::BBTrack`) are actually blocks within the song editor, not within the BB editor.

All tracks in BB editor are located within a separate track container `BBTrackContainer`. You can access the container by `Engine::getBBTrackContainer()`. Playing a certain BB track (i.e. all TCOs of the tracks inside at a certain position) is achieved via `BBTrack::play(...)`.

Tracks in BB editor has one TCO per each beat/bassline. The index of BB track corresponds to the index of TCO for parent BB track in the track.

# Mixer and audio rendering
LMMS has a class for audio rendering, named `Mixer`. `Mixer::renderNextBuffer` contains the main functionality of `Mixer`. Major works that the function does:
1. Remove all finished `PlayHandle`s, which will be explained
2. Call `Song::processNextBuffer` to play songs and add `PlayHandle`s
3. Queue jobs for `MixerWorkerThread` and wait for worker threads (`PlayHandle`, effects in instrument tracks/sample tracks, FX mixing)
4. Call `runChangesInModel` to process pending non-automated changes

## Mixer worker threads
`MixerWorkerThread` processes `ThreadableJob`s queued in `Mixer::renderNextBuffer`. There are three kinds of jobs that `MixerWorkerThread` runs.
- `AudioPort`: Processes intermediate audio signals(Currently used for audio effects for tracks)
- `FxChannel`: Processes audio effects per FX channel
- `PlayHandle`: Plays notes/instruments/audio samples

## Synchronization with the mixer

One goal is that the mixer runs normally without using locks. The mixer will lock at an appropriate moment when changes to the song are requested, such as removing tracks and changing sample files, avoiding the use of freed data.

When a non-automated change to the song is needed, instead of calling `lock()` and `unlock()` on a mutex, the mixer functions `requestChangeInModel()` and `doneChangeInModel()` are used. These functions synchronize GUI threads to do changes when the mixer deems appropriate. The functions do nothing if they are called from the mixer main thread. If they were called from a mixer worker thread, that would be a design error.