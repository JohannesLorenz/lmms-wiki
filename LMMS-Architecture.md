# Introduction

There should be some documentation about LMMS' core architecture and concepts 
to help newcomers with some development skills to understand how it works.

This page is an attempt to aggregate the informations found.

# Model and ModelView

LMMS uses a design pattern similar to [Model–view–controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller). In LMMS, most of views also play a role as controllers.
Model emits `Model::dataChanged` signal when its data is changed and `Model::propertiesChanged` when its property(range, step size, etc.). These signals are used for updating corresponding views(see `ModelVies::doConnections`) and tracking changes in models.

`ModelView` holds reference to its model. It *doesn't own the model* except the model is default-constructed. Existing `ModelView` instance can be reused by setting new model using `ModelView::setModel`. Views handle model change by overriding `modelChanged()`.

# BB tracks

A list of TCOs(`TrackContentObjects`) by calling `Track::getTCOs` for beat/bassline(BB) tracks(type `Track::BBTrack`) are actually blocks within the song editor, not within the BB editor.

All tracks in BB editor are located within a separate track container `BBTrackContainer`. You can access the container by `Engine::getBBTrackContainer()`. Playing a certain BB track (i.e. all TCOs of the tracks inside at a certain position) is achieved via `BBTrack::play(...)`.

Tracks in BB editor has one TCO per each beat/bassline. The index of BB track corresponds to the index of TCO for parent BB track in the track.

# Linux audio architecture

Linux audio architecture today consists of three pieces: ALSA, PA and Jack.

ALSA is the low-level kernel implementation that provides support for
all hardware devices. The hardware support there is as good or bad as
all other hardware support on Linux - it varies. But that's not any kind
of fundamental problem in Linux audio per se - it's just a problem of
hardware vendors not caring about Linux, and as with any other hardware,
it's up to the user to select hardware that is known to work well under
Linux.

Both PA and Jack are higher-level architectures which use ALSA primarily
as a backend. Their job is not to "replace" ALSA - they couldn't,
because they can't work without a low-level backend that deals with the
hardware directly. The job of ALSA is to abstract away the hardware so
that other applications can use it. PA and Jack both have different
purposes and fulfill different functions. None of them are meant as any
kind of attempt at replacing each other.

# Synchronization with the mixer

The mixer is the element that renders the song into audio frames. One goal is that the mixer runs normally without using locks. The mixer will lock at an appropriate moment when changes to the song are requested, such as removing tracks and changing sample files, avoiding the use of freed data.

When a non-automated change to the song is needed, instead of calling `lock()` and `unlock()` on a mutex, the mixer functions `requestChangeInModel()` and `doneChangeInModel()` are used. These functions synchronize GUI threads to do changes when the mixer deems appropriate. The functions do nothing if they are called from the mixer main thread. If they were called from a mixer worker thread, that would be a design error.