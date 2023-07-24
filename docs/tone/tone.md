---
layout: default
title: Tone
level: 0
order: 300
---

A `tone` is analogous to a mark in a visualization.
It is a unit sound that has no puase (except the pause between two tapping sounds) in it.
A sound point is each point that encodes the corresponding data point.
For instance, data points `[{x: 1, y: 2}, {x: 2, y: 3}]` can be mapped to
an audio graph of `[{time: 0.5, pitch: 300}, {time: 1, pitch: 320}]`.
Then each element in the audio graph is a sound point.
A `tone` object has the following properties.

## Tone properties

| Channel | type | Description |
| ------- | ---- | ----------- |
| `type` | `string` or `object` | (Default: `'default'`, a sine wave oscillator) The instrument type of a tone. |
| `continued` | `boolean` | (Default: `false`) Whether a tone is continued (no break between sound points) or discrete (breaks between sound points). This property can be ignored for sampled instruments (see below). |
| `sample` | `object` | A sampling object. |

### Supported instrument types

#### By naming an instrument

##### Osicllator types (see [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode/type))

These instrument types support `continued` tones.

- `'default'`: default oscillator (`'sine'` )
- `'square'`: square oscillator
- `'sawtooth'`: default oscillator (`'sawtooth'` form)
- `'triangle'`: default oscillator (`'triangle'` form)

##### Muliti-Pitch Instrumental types

These instrument types do *NOT* suppport `continued` tones.

- `'piano'`: square oscillator
- `'pianoElec'`: square oscillator
- `'violin'`: square oscillator
- `'metal'`: square oscillator
- `'guitar'`: square oscillator

##### Single-Pitch Instrumental types

These instrument types do *NOT* suppport `continued` tones.
Note that a `pitch` encoding technically works, but the quality will not be ideal.

- `'hithat'`: square oscillator
- `'highKick'`: square oscillator
- `'lowKic'`: square oscillator
- `'clap'`: square oscillator

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

#### Providing a Periodic Wave form

It is possible to provide a periodic wave form (see [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicWave))
by providing a periodic wave object.
These instrument types suppport `continued` tones.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "tone": {
    "type": {
      "real": [ ... ], // sine terms
      "imag": [ ... ] // cosine terms
    }, 
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
let periodicWave = {
  "real": [ ... ], // sine terms
  "imag": [ ... ] // cosine terms
};
let tone = new Erie.Tone(periodicWave);
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>


#### Sampling a sound

See the sampling documentation.

### About supporting `continued` tone types

Erie offers those instruments by sampling audio files for notes.
Due to the limitations in Web Audio API,
It is not possible to continue sampled (i.e., non-oscillator) sounds.
Thus, either a periodic form or oscillator-type instruments can be set as a continued tone.
