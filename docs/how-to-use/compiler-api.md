---
layout: default
title: Compiler API
parent: How to use
level: 1
order: 104
---

Erie's compiler (in JS) returns a playable audio stream object.

## Global

### `async compileAudioGraph(spec) -> SequenceStream`

This async function returns a `Promise` which is resolved to a `SequenceStream` instance.

## `SequenceStream` class

This class is an interface to player.
Note that the `SequenceStream` is private, and not directly accessible.

### `SequenceStream.queue`

This property is an `AudioQueue` instance that is prerendered.

### `async SequenceStream.prerender()`

This method generates an `AudioQueue` that is ready to be played and returns a `Promise` which is resolved to that prerendered `AudioQueue` instance.

### `async SequenceStream.playQueue()`

This method plays the specified sonification, and then returns a `Promise` which is resolved to `undefined`.

### `async SequenceStream.stopQueue()`

This method stops the specified sonification, and then returns a `Promise` which is resolved to `undefined`.
