# Automation Internals

This page tries to explain how the LMMS code handles automation patterns and controllers.

## Basic definitions

* `Model`s are the base classes for all data represented by widgets like knobs, sliders, spinboxes...
* `Model`s that can be connected to `AutomationPattern`s or `Controller`s are `AutomatableModel`s
* `AutomationPattern` contains a list of all `AutomatableModel's` connected (internally named `m_objects`). `AutomationPattern` is a `TrackContentObject` (TCO).

* `AutomationTrack`s contain data for one automation track, represented as `AutomationTrackView` in the UI. This is basically
  a whole automation track row in the Song Editor.
* An AutomationTrack can contain multiple `AutomationPattern`s, represented as `AutomationPatternView`s in the UI.

## Automation through Controllers

Controller automation is simple: The Models of the widgets have the controller linked via pointers. A call to `AutomatableModel::value()` returns

* the Controller's value (recalculated to fit the Model using `controllerValue`), or else
* the non-automated `m_value`

## Automation through Automation Patterns

Automation through patterns requires pre-calculation by the `Song` class, as these patterns can be recorded, or require interpolation.

* When an `AutomationTrackView::dropEvent` occurs,
  1. the ID of the `AutomatableModel` is provided in the drop event
  2. the `AutomationTrackView` looks up that ID in the `Engin::projectJournal` to get the `AutomatableModel`
  3. the `AutomationTrack` creates a new `AutomationPattern`
  4. the AutomatableModel gets added to the new `AutomationPattern`
  5. the connection is now done
* When a Song is played, `Song::processNextBuffer` calls `Song::processAutomations`, which
  1. calls `automatedValuesAt`, i.e. returns all `AutomatableModel`s at the current position in a QMap<AutomatableModel*, float> by using the `AutomationPattern` members `objects()` and `valueAt()`
  2. calls all `AutomatableModel::setAutomatedValue` with the pair of the QMap, i.e. assigns floats to the `AutomatableModel`s' static `m_value`s

## Automation and savefiles

Automation is usually done by saving an XML node "automationpattern" (inside a node "automationtrack") which saves all connected models as "object" nodes. Each such node specifies an "id" attribute. `AutomatableModel` also have such an "id" attribute, so they can be matched in the journalling system. The `AutomatableModel` usually is stored as a node having its name, or as a node "quoted-model" with an attribute "quoted-name".

When loading a savefile, LMMS can load automation patterns and automatable models in arbitrary order. When loading a pattern's object node, a variable `AutomationPattern::m_idsToResolve` gets filled with the journalling IDs. After the full song has been loaded, `AutomationPattern::resolveAllIDs()` adds model pointers to the `AutomationPattern::m_objects` array.

Now, when `AutomationPatternView::paintEvent` is being called to draw a pattern view, the minimum and maximum value are fetched directly from the first object (i.e. the first `AutomatableModel`) of the `AutomatablePattern`.
