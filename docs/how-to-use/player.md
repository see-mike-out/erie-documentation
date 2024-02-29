---
layout: default
title: Player API
parent: How to use
level: 1
order: 105
---


## `AudioQueue` class

This class stores a flattened, scheduled queue of sounds to be played.

### How queue elements are stored

Each sub-queue item of an `AudioQueue` consists of sound elements with their physical values to be played.

At high-level, there are four types of sub-queues: `Speech`, `Tone`, `Tone-Speech`, and `Tone-Overlay`.

1. `Speech` is played using `SpeechSynthesis` API.
2. `Tone` is played using `AudioContext` API.
3. `Tone-Speech` uses both `SpeechSynthesis` and `AudioContext`.
4. `Tone-Overlay` is comprised of multiple `Tone`-type sub-queues.

This distnction depends on how those APIs work.

### `async AudioQueue.play(start, end, options) -> Promise<undefined|[buffer]>`

This methods plays the queued sonification. `SequenceStream.playQueue()` runs this method.
If `start` is specified, the sonfication is played from the `start`-th queue element.
If `start` and `end` are both specified, the sonfication is played from the `start`-th queue element to `(end - 1)`-th queue element.

The `options` object can take the following property:

- `pcm` (`boolean`, default = `false`, optional): If set `true`, the resulting `Promise` is resolved to an *Array* of`AudioPrimitiveBuffer` instances (see below) and no sound will be played. If set `false` (default), the resulting `Promise` is resolved to `undefined`.

### `AudioQueue.stop() -> undefined`

This immediately stops the sonification being played.

### `AudioQueue.pause() -> undefined`

This immediately pauses the sonification being played.

### `async AudioQueue.resume() -> Promise<undefined>`

This resumes playing the sonification from where it was stopped.
If it was paused at `i`-th queue element, it starts playing from the `i`-th element.

### `AudioQueue.playAt`

This gives the index of the sub-sequence that is currently being played.

### `AudioQueue.getFullAudio(ttsFetchFunction) -> Promise<Blob>`

This method generates an `mp3` file for the full audio.
If there is any `text` type or `tone-speech-series` type queue item, `ttsFetchFunction` must be provided.
Otherwise, there will be a `TypeError`.
A `ttsFetchFunction` must take a queue item as an input, and should return an `ArrayBuffer`.
For an exmaple `ttsFetchFunction`, refer to ['Using Google Cloud TTS'](google-tts.html).

Note that this method only works on a client side (i.e., can't be run on a Node server), however your `ttsFetchFunction` may need to be run only on a server.
In that case, use `fetch` method.
While `audioQueueCompile` function should work on server side, most of the methods of an `AudioQueue` instance does not work on a server because niether `AudioContext` nor `OfflineAudioContext` is not supported on Node (only on supported browers).
Therefore, the following procedures are suggested.

1. (On Client) Compile your specification to an `AudioQueue` instance.
2. (On Server) Define an API endpoint for getting TTS audio. In this case, you can use `GoogleCloudTTSGenerator` as an interface between Erie and Google Cloud TTS.
3. (On Client) Define a `ttsFetchFunction` function to the TTS API endpoint.
4. (On Client) Run this `getFullAudio` method with the `ttsFetchFunction` function.

#### An exmaple after the ['Using Google Cloud TTS'](google-tts.html) example

{% highlight html %}
<script>
  let BlobLink;
  async function getFullAudio() {
    BlobLink = await queue.getFullAudio(getTTS);
  }
</script>
...
<a href={totalBlobLink}>Download</a>
...
{% endhighlight %}

## `AudioPrimitiveBuffer` class

This class contains information about the `AudioBuffer` for a queue item.
An `AudioPrimitiveBuffer` instance has the following attributes:

- `length`: The length of the sound in seconds
- `sampleRate` (default `44100`): The sample rate of the buffer.
- `compiled`: Whether the final buffer has been compiled.
- `compiledBuffer`: An `AudioBuffer` instance that contains the buffer data.
- `primitive`: An array that contains initially collected data. Each element has `at` (where this sound locates in seconds) and `data` an `AudioBuffer` from position 0.

## `async makeWaveFromBuffer(buffer: AudioBuffer, ext: Extension) -> Promise<Blob>`

This function takes an audio buffer and resolves a `Blob` object for an audio.
The `ext` can be `mp3`, `wav`, etc. (default: `wav`)
If it is `$raw`, then it returns an `ArrayBuffer` for the wav audio file.

### Example

{% highlight html %}
<script>
  function getBlobFromAudioBuffer(buffers, i) {
    let blobLink = []
    for (const b of buffers) {
      if (b?.constructor.name === Erie?.AudioPrimitiveBuffer?.name) {
        Erie.makeWaveFromBuffer(b.compiledBuffer, "mp3").then((blob) => {
          blobLink[i] = window.URL.createObjectURL(blob);
          i++;
        });
      }
    }
    return blobLink;
  }
  audio?.queue
    ?.play(0, 1, { pcm: true })
    .then((buffer) => {
      getBlobFromAudioBuffer(buffer, i);
    });
</script>
...
<a href={blobLink[i]}>Download</a>
...
{% endhighlight %}

## Player Events

As a player executes each queue element, it fires the following events.
These events are primarliy designed for scheduling the recorder (a Chrome extension).

### Event `erieOnPlayQueue`

This event is fired when an `AudioQueue` starts playing (applies when it is resumed).
`event.detail` contains `pid` (a 6-letter random string ID for the played sonficiation).

### Event `erieOnFinishQueue`

This event is fired when an `AudioQueue` stops playing (applies when it is force-stopped or paused).
`event.detail` contains `pid` (a 6-letter random string ID for the played sonficiation).

### Event `erieOnPlayTone`

This event is fired when an `AudioQueue` starts playing an `Tone` item/element.
Note that for a `Tone-Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Tone`).

### Event `erieOnFinishTone`

This event is fired when an `AudioQueue` stops playing an `Tone` item/item.
Note that for a `Tone + Speech` element, this event can be fired multiple times.
`event.detail` contains `sid` (a 6-letter random string ID for the played `Tone`).

### Event `erieOnNotePlay`

This event is fired whenever a tone or speech is played.
`event.detail` has `type` and `note` property.
This could be useful to match with sonification and visualization (refer to these Svelte files ([1](https://github.com/see-mike-out/erie-editor/blob/1d9ee242457a6be5bf6f24ba906cd22b1ac5f9dd/src/tester-components/visualization-view.svelte#L52-L60) and [2](https://github.com/see-mike-out/erie-editor/blob/main/src/chart-control/highlight-mark.js)) to see how it can work).

- `type`: either `tone` or `speech`.
- `note`
  - For `tone` type, `note` contains auditory values and the `__datum` property for the datum that it encodes. (Note: for a continuous tone, it only includes the first datum).
  - For `speech` type, `note` includes the speech text and other auditory information (e.g., speech rate, language, etc.).

### Event `erieOnNoteStop`

This event is fired whenever a tone or speech is finished.
`event.detail` has `type` and `note` property (same as above).

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
