# Introduction

There should be some documentation about LMMS' core architecture and concepts 
to help newcomers with some development skills to understand how it works.

This page is an attempt to aggregate the informations found.

# Model and ModelView

LMMS uses a design pattern similar to [Model–view–controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller). In LMMS, most of views also play a role as controllers.
Model emits `Model::dataChanged` signal when its data is changed and `Model::propertiesChanged` when its property(range, step size, etc.). These signals are used for updating corresponding views(see `ModelVies::doConnections`) and tracking changes in models.

`ModelView` holds reference to its model. It *doesn't own the model* except the model is default-constructed. Existing `ModelView` instance can be reused by setting new model using `ModelView::setModel`. Views handle model change by overriding `modelChanged()`.

# BB tracks

**Question:**

From a song, you can get a list of tracks, and some of those tracks are of
type track::BBTrack.

I can get a list of TCOs (track->getTCOs) and these are
trackContentObjects.  I've been confused because I thought TCOs were the
"tracks within the BB track", meaning for example, a kick, snare and high
hat.  But they are actually blocks within the song editor.  If I have three
measures in the BBTTrack, I'll have three trackContentObjects when I debug.

I want to access the tracks (in my example they would be InstrumentTracks)
that live within the BBTrack, and I want to do it from the BBTrack object
 that is returned from the Song.

Would someone give me a quick overview of the BB track architecture?

**Answer:**

All actual tracks of BB-tracks are located within BbTrackContainer
(see bb_track_container.h/bb_track_container.cpp). Get all tracks via
engine::getBBTrackContainer()->tracks(). Playing a certain BB track
(i.e. all TCOs of the tracks inside at a certain position) is achieved
via bbTrack::play(...).

BB track 0 = all TCOs (patterns) of the instrument tracks inside at
position 0. BB track 1 = all patterns at position 1 and so on.. See
bbTrackContainer::fixIncorrectPositions() for an illustration.

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