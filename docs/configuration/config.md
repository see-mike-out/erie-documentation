---
layout: default
title: Config
level: 0
order: 900
---

A `config` object specifies global controls over a `stream`, using the follwing properties.

## `Config` properties

| Property | type | Description |
| -------- | ---- | ----------- |
| `speechRate` | `number` | (Default: `1.75`) Overall default speech rate (how fast a speech sound is). `1` is the normal speed, yet `1.75`—`2` is quite common in a screen reader setting. |
| `skipScaleSpeech` | `boolean` | (Default: `false`) Wheter to skip playing the scale descriptions. |
| `skipDescription` | `boolean` | (Default: `false`) Wheter to skip playing the description. |
| `skipTitle` | `boolean` | (Default: `false`) Wheter to skip playing the title. |
| `overlayScaleConsistency` | `boolean|object` | (Default: `true`) Whether to use common scales for `overlay` compositions (if channels is the same `type` and encodes the same datset). It is also possible to set by each encoding channel. |
| `forceOverlayScaleConsistency` | `boolean|object` | (Default: `false`) Force using common scales for `overlay` compositions even if they encode different datasets. It is also possible to set by each encoding channel. |
| `sequenceScaleConsistency` | `boolean|object` | (Default: `true`) Whether to use common scales for `sequence` compositions (if channels is the same `type` and encodes the same datset). It is also possible to set by each encoding channel. |
| `forceSequenceScaleConsistency` | `boolean|object` | (Default: `false`) Force using common scales for `sequence` compositions even if they encode different datasets. It is also possible to set by each encoding channel. |
| `timeUnit` | `object` | The unit for time (see below). |

For nested specifications (e.g., a `sequence` of `overlay` and single streams),
the closest `config` object has the priority.

For the behavior of `overlayScaleConsistency` and `sequenceScaleConsistency`,
refer to the `encoding` documentation.

### `timeUnit` property

You can set the times by the exact seconds or by their beat counts.
In `config`, the `timeUnit` property lets you change that.

This applies to `time`, `time2`, `duration`, `tapCount`, and `tapSpeed` channels.
For instance, when `timeUnit.unit` is `'beat'`, a `tapSpeed` channel's unit is tappings per beat.

| Property | type | Description |
| -------- | ---- | ----------- |
| `unit` | `'second'|'beat'` | (Default: `'second'`) The unit for time. `'second'`: exact second amount. `'beat'`: beat counts. |
| `tempo` | `number` | (Default: `100`) Beat per minute. Only works when the `unit` is `'note'`. |
| `rounding` | `'always'|'start'|'never'` | (Default: `'always'`) Whether to round the time values (start/end times) to specified beat counts `'always'`: both start and end, `'start'`: only for the start time, `'never'`: no rounding. Only works when the `unit` is `'beat'`. |
| `roundingBy` | `'number'` | (Default: `1`) The beath counts for rounding (precision). |

#### More notes on tempo and rounding

The tempo unit is bpm (beat per *minute*).
A beat count is multipled by 60 / `tempo` (the resulting unit is seconds).
Suppose when the `timeUnit.unit` is `'note'`, `tempo` is `100` bpm, and `baseBeat` is `1`.
Then, a time value of `3` (beats) is converted to 3 * 60 / 100 = 1.8 seconds.