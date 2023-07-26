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
| `overlayScaleConsistency` | `boolean` | (Default: `true`) Whether to use common scales for `overlay` compositions. |
