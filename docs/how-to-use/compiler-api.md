---
layout: default
title: Compiler API
parent: How to use
level: 1
order: 104
version: current
---

Erie.js compiler (in JS) returns a playable audio stream object.

## Global

### `async compileAudioGraph(spec) -> Promise<SequenceStream>`

This async function returns a `Promise` which is resolved to a `SequenceStream` instance.

## `SequenceStream` class

This class is an interface to Erie.js web player.
Note that the `SequenceStream` is a private class, and not directly accessible.

### `SequenceStream.queue`

This property is an `AudioQueue` instance that is prerendered.

### `async SequenceStream.prerender() -> Promise<AudioQueue>`

This method generates an `AudioQueue` that is ready to be played and returns a `Promise` which is resolved to that prerendered `AudioQueue` instance.
For the `AudioQueue` class, please refer to the player documentation.

### `async SequenceStream.playQueue() -> Promise<undefined>`

This method plays the specified sonification, and then returns a `Promise` which is resolved to `undefined`.

### `SequenceStream.stopQueue()`

This method stops the specified sonification.

### `SequenceStream.queue.playAt`

This gives the index of the sub-sequence that is currently being played.
