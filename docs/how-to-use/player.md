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

### `async AudioQueue.play(start, end) -> Promise<undefined>`

This methods plays the queued sonification. `SequenceStream.playQueue()` runs this method.
If `start` is specified, the sonfication is played from the `start`-th queue element.
If `start` and `end` are both specified, the sonfication is played from the `start`-th queue element to `(end - 1)`-th queue element.
The resulting `Promise` is resolved to `undefined`.

### `AudioQueue.stop() -> undefined`

This immediately stops the sonification being played.

### `AudioQueue.pause() -> undefined`

This immediately pauses the sonification being played.

### `async AudioQueue.resume() -> Promise<undefined>`

This resumes playing the sonification from where it was stopped.
If it was paused at `i`-th queue element, it starts playing from the `i`-th element.

### `AudioQueue.playAt`

This gives the index of the sub-sequence that is currently being played.

### `AudioQueue.queue[i].getPCM()` or `generatePCMCode(AudioQueue.queue[i])`(experimental)

This method returns an [`AudioBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer) object with [Raw Pulse-code Modulation](Pulse-code modulation).
You can run this method for a `tone-series` queue item.
Currently, this is an experimental feature and only supports a sinusoidal oscillator with time, pitch, pan, and loudness channels.
It works well for a discrete `tone-series` queue. Continuous queues are not rendered smooothly.
Refer to the below usage

{% highlight js %}
compileAudioGraph(spec).then((audio) => {
  audio.prerender().then((queue) => {
    queue[i].getPCM()?.then((buffer) => {
      if (buffer) {
        let ctx = new AudioContext();
        let source = ctx.createBufferSource();
        source.buffer = buffer;
        source.connect(ctx.destination);
        source.start();
        source.onended = () => {
          console.log("done");
        };
      }
    });
  });
})
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
