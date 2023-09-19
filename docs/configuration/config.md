---
layout: default
title: Config
level: 0
order: 900
---

A `config` object specifies global controls over a `stream`, `sequence`, and `overlay`, using the follwing properties.

## `Config` properties

| Property | type | Description |
| -------- | ---- | ----------- |
| `speechRate` | `Number` | (Default: `1.75`) Overall default speech rate (how fast a speech sound is). `1` is the normal speed, yet `1.75`—`2` is quite common in a screen reader setting. |
| `skipScaleSpeech` | `Boolean` | (Default: `false`) Wheter to skip playing the scale descriptions. |
| `playScaleAt` | `'beforeAll'|'beforeThis'|'afterAll'|'afterThis'` | (Default: `beforeThis`) Where to play the scale descriptions. `'beforeAll'` (before all the sub-streams), `'beforeThis'` (before the specified stream), `'afterAll'` (after all the sub-streams), `'afterThis'` (after the specified stream). |
| `skipDescription` | `Boolean` | (Default: `false`) Wheter to skip playing the description. |
| `skipTitle` | `Boolean` | (Default: `false`) Wheter to skip playing the title. |
| `skipStartSpeech` | `Boolean` | (Default: `false`) Wheter to skip the very first intro speech, "To stop playing the sonificatoin, press the X key." |
| `skipStartPlaySpeech` | `Boolean` | (Default: `false`) Wheter to skip playing the start of the audio speech, "Play sonification." |
| `skipFinishSpeech` | `Boolean` | (Default: `false`) Wheter to skip playing the end of sonification speech, "Finished." |
| `skipSquenceIntro` | `Boolean` | (Default: `false`) Wheter to skip playing the introduction of a sequence, "This sonification sequence consists of {the number of streams} parts." |
| `skipSequenceTitle` | `Boolean` | (Default: `false`) Wheter to skip playing the title of each sequence element. |
| `skipSequenceDescription` | `Boolean` | (Default: `false`) Wheter to skip playing the title of each sequence element. |
| `skipOverlayIntro` | `Boolean` | (Default: `false`) Wheter to skip playing the introduction of an overlay, "{determiner: this/the first/the second...} stream has {the number of overlays} overlaid sounds." |
| `skipOverlayTitle` | `Boolean` | (Default: `false`) Wheter to skip playing the title of each overlay element. |
| `skipOverlayDescription` | `Boolean` | (Default: `false`) Wheter to skip playing the title of each overlay element. |
| `overlayScaleConsistency` | `Boolean|Object` | (Default: `true`) Whether to use common scales for `overlay` compositions (if channels is the same `type` and encodes the same datset). It is also possible to set by each encoding channel. |
| `forceOverlayScaleConsistency` | `Boolean|Object` | (Default: `false`) Force using common scales for `overlay` compositions even if they encode different datasets. It is also possible to set by each encoding channel. |
| `sequenceScaleConsistency` | `Boolean|Object` | (Default: `true`) Whether to use common scales for `sequence` compositions (if channels is the same `type` and encodes the same datset). It is also possible to set by each encoding channel. |
| `forceSequenceScaleConsistency` | `Boolean|Object` | (Default: `false`) Force using common scales for `sequence` compositions even if they encode different datasets. It is also possible to set by each encoding channel. |
| `timeUnit` | `Object` | The unit for time (see below). |

For nested specifications (e.g., a `sequence` of `overlay` and single streams),
the closest `config` object has the priority.

For the behavior of `overlayScaleConsistency` and `sequenceScaleConsistency`,
refer to the `encoding` documentation.

### `timeUnit` property

It is possible to set the time unit to seconds (default) or beat counts using the `timeUnit` property in `config`.

This applies to `time`, `time2`, `duration`, `tapCount`, and `tapSpeed` channels.
For instance, when `timeUnit.unit` is `'beat'`, a `tapSpeed` channel's unit is tappings per beat.

| Property | type | Description |
| -------- | ---- | ----------- |
| `unit` | `'second'|'beat'` | (Default: `'second'`) The unit for time. `'second'`: exact second amount. `'beat'`: beat counts. |
| `tempo` | `Number` | (Default: `100`) Beat per minute. Only works when the `unit` is `'beat'`. |
| `rounding` | `'always'|'start'|'never'` | (Default: `'always'`) Whether to round the time values (start/end times) to specified beat counts `'always'`: both start and end, `'start'`: only for the start time, `'never'`: no rounding. Only works when the `unit` is `'beat'`. |
| `roundingBy` | `Number` | (Default: `1`) The beath counts for rounding (precision). |

#### More notes on tempo and rounding

The tempo unit is bpm (beat per *minute*).
A beat count is multipled by 60 / `tempo` (the resulting unit is seconds).
Suppose when the `timeUnit.unit` is `'note'`, `tempo` is `100` bpm, and `baseBeat` is `1`.
Then, a time value of `3` (beats) is converted to 3 * 60 / 100 = 1.8 seconds.
