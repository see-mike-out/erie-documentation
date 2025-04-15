---
layout: future
title: Streaming API
parent: How to use
level: 1
order: 106
version: future
---

## `StreamingStream` class

This class is an interface to Erie.js web player for streaming data as an output of the `compileAudioGraph` function.
This is also a private class, and not directly accessible.

### Public Attributees

| Attribute | Type | Description |
|-----------|------|-------------|
| `is_relative` | `Boolean` | Whether the `time` channel has `relative` timing option. |
| `is_continued` | `Boolean` | Whether the `tone` of this stream has continued sound. |
| `has_base_tone` | `Boolean` | Whether this stream has a base tone (some underlying sound keeps playing). |
| `status` | `'Stopped'|'Playing'|'Finished'` | The status of ths player (not the object). |
| `is_destroyed` | `Boolean` | Whether the stream is destroyed (i.e., the instrument expired). |
| `is_started` | `Boolean` | Whether the stream is started (i.e., base tone created). |
| `is_muated` | `Boolean` | When this stream has a base tone, whether it is muted or not. |
| `history` | `Array<{time: Date, data: Datum[]}>` | The history of played data points so far. This array only stores up to the specified number (default: `100`) of data instances. |

### Public Methods

#### `start() -> void`

This method sets the base tone when it is specified, and creates a new `AudioContext` for streaming.
For streams without base tone, it does nothing audible, but it is still important to run this method to signal that this stream has started receiving data points.

#### `destroy() -> void`

This method expires the `AudioContext` for the stream and stops the base tone if there is one.
Once you run this method, you cannot run the below methods. To reuse the stream, you will need to run the `start` method again.
This does not remove the history.

#### `play(data: Datum[], test?: boolean, playback_query?: PlaybackQuery) -> void`

Play received data points (`data`, in tidy format---an array of objects).
It is up to your application design in terms of how you trigger feeding data.
Once the `data` is played, it is stored in the history with a timestamp.

#### `async play_test() -> Promise<void>`

This method plays a test data if specified.

#### `async stop()`/`async cancel() -> Promise<void>`

This method stops playing any "data". It does not close the base tone.
Yet, it still saves the received data in the history.

#### `muteBaseTone() -> void`

Mute the base tone if there is one.

#### `unmuteBaseTone() -> void`

Unmute the base tone if there is one.
