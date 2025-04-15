---
layout: default
title: Tone
level: 0
order: 500
version: current
---

A `tone` is analogous to a mark in a visualization.
It is a unit sound that has no puase (except the pause between two tapping sounds) in it.
A sound point is each point that encodes the corresponding data point.
For instance, data points `[{x: 1, y: 2}, {x: 2, y: 3}]` can be mapped to
an audio graph of `[{time: 0.5, pitch: 300}, {time: 1, pitch: 320}]`.
Then each element in the audio graph is a sound point.
A `tone` object has the following properties.

## Tone properties

| Property | type | Description |
| -------- | ---- | ----------- |
| `type` | `String` or `Object` | (Default: `'default'`, a sine wave oscillator) The instrument type of a tone. |
| `continued` | `Boolean` | (Default: `false`) Whether a tone is continued (no break when audio property changes) or discrete (breaks). This property can be ignored for sampled instruments by defaulting to `'false'` (see below). |
| `filter` | `Array[String]` | A list of audio filter names. |

### Supported instrument types

#### By naming an instrument

##### Osicllator types (see [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode/type))

These instrument types support `continued` tones.

- `'default'`: default oscillator with a `'sine'` wave form
- `'square'`: default oscillator with a `'square'` wave form
- `'sawtooth'`: default oscillator with a `'sawtooth'` wave form
- `'triangle'`: default oscillator with a `'triangle'` wave form

##### Noise types

These instrument types support `continued` tones.

- `'whiteNoise'`: random noise (random buffer from -1 to 1)
- `'brownNoise'`: Brownian/red/random-walk noise ([definition](https://en.wikipedia.org/wiki/Brownian_noise))
- `'pinkNoise'`: fractal noise ([defintion](https://en.wikipedia.org/wiki/Pink_noise))

##### Muliti-Pitch Instrumental types

These instrument types do *NOT* suppport `continued` tones.
*Why?* These instruments are technically sampled from different audio files, and they can't be smoothly connected.

- `'piano'`: classical piano
- `'pianoElec'`: electric piano keyboard
- `'violin'`: classical violin
- `'metal'`: metal guitar
- `'guitar'`: classical guitar

##### Single-Pitch Instrumental types

These instrument types do *NOT* suppport `continued` tones.
Note that a `pitch` encoding technically works, but the quality will not be ideal.

- `'hithat'`: hit-hat sound
- `'highKick'`: high-kick sound
- `'lowKick'`: low-kick sound
- `'clap'`: clap sound

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "tone": {
    "type": "default", // default value
    "continued": false, // default value
   },
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let tone = new Erie.Tone("default", false);
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>

#### Sampling a sound, using a synth, or providing a Periodic Wave form

Set the `type` as the name of a sampled sound, synth instrument, or periodic wave as defined in the top-level `sampling`, `synth`, or `wave` object.
Make sure to not use the overlapping names.
Erie searches the instrument by looking at the named instrument list, the `wave` object, the `synth` object, and the `sampling` object, in this order.

### About supporting `continued` tone types

Erie offers those instruments by sampling audio files for notes.
Due to the limitations in Web Audio API,
It is not possible to continue sampled (i.e., non-oscillator) sounds.
Thus, either a periodic form or oscillator-type (including synth) instruments can be set as a continued tone.

#### Attaching audio filters

To attach audio filters, one can provide the name of filters. See the `filter` document.