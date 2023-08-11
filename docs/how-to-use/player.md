---
layout: default
title: Player API
parent: How to use
level: 1
order: 105
---


## `AudioQueue` class

This class stores a flattened queue of sounds to be played.

### How queue elements are stored

Each element in an `AudioQueue` is stored as sound items with their physical values to be played.

At high-level there are three types: `Speech`, `Tone`, and `Speech + Tone`.

1. `Speech` is played using `SpeechSynthesis` API.
2. `Tone` is played using `AudioContext` API.
3. `Speech + Tone` uses both `SpeechSynthesis` and `AudioContext`.

This distnction is made because those APIs fucntion in different ways.

### `async AudioQueue.play(start, end) -> Promise(undefined)`

This methods plays the queued sonification. `SequenceStream.playQueue()` runs this method.
If `start` is specified, the sonfication is played from the `start`-th queue element.
If `start` and `end` are both specified, the sonfication is played from the `start`-th queue element to `(end - 1)`-th queue element.
The resulting `Promise` is resolved to `undefined`.

### `AudioQueue.stop() -> undefined`

This immediately stops the sonification being played.

### `AudioQueue.pause()`

This immediately pauses the sonification being played.

### `async AudioQueue.resume() -> Promise(undefined)`

This resumes playing the sonification from where it was stopped.
If it was paused at `i`-th queue element, it starts playing from the `i`-th element.

## Player Events

As a player executes each queue element, it fires the following events

### Event `erieOnPlayQueue`

This event is fired when an `AudioQueue` starts playing (applies when it is resumed).
`event.detail` contains `pid` (a 6-letter random string ID for the played sonficiation).

### Event `erieOnFinishQueue`

This event is fired when an `AudioQueue` stops playing (applies when it is force-stopped or paused).
`event.detail` contains `pid` (a 6-letter random string ID for the played sonficiation).

### Event `erieOnPlayTone`

This event is fired when an `AudioQueue` starts playing an `Tone` item/element.
Note that for a `Tone + Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Tone`).

### Event `erieOnFinishTone`

This event is fired when an `AudioQueue` stops playing an `Tone` item/item.
Note that for a `Tone + Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Tone`).

### Event `erieOnPlaySpeech`

This event is fired when an `AudioQueue` starts playing an `Speech` item/element.
Note that for a `Tone + Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Speech`) and `sound` (the played speech text information).

### Event `erieOnFinishSpeech`

This event is fired when an `AudioQueue` stops playing an `Speech` item/item.
Note that for a `Tone + Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Speech`).

### Event `erieOnStatusChange`

This event is fired together with the above six events.
`event.detail` contains `status`.

- `'started'`: when `erieOnPlayQueue` is fired.
- `'finished'`: when `erieOnFinishQueue` is fired.
- `'tone-started'`: when `erieOnPlayTone` is fired.
- `'tone-finished'`: when `erieOnFinishTone` is fired.
- `'speech-started'`: when `erieOnPlaySpeech` is fired.
- `'speech-finished'`: when `erieOnFinishSpeech` is fired.
