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
| `overlayScaleConsistency` | `boolean` | (Default: `true`) Whether to use common scales for `overlay` compositions (if channels is the same `type` and encodes the same datset). |
| `forceOverlayScaleConsistency` | `boolean` | (Default: `false`) Force using common scales for `overlay` compositions even if they encode different datasets. |
| `sequenceScaleConsistency` | `boolean` | (Default: `true`) Whether to use common scales for `sequence` compositions (if channels is the same `type` and encodes the same datset). |
| `forceSequenceScaleConsistency` | `boolean` | (Default: `false`) Force using common scales for `sequence` compositions even if they encode different datasets. |

For nested specifications (e.g., a `sequence` of `overlay` and single streams),
the closest `config` object has the priority.

For the behavior of `overlayScaleConsistency` and `sequenceScaleConsistency`,
refer to the `encoding` documentation.