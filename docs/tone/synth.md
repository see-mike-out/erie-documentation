---
layout: default
title: Synth
parent: Tone
level: 1
order: 503
---

To use an FM/AM synth tone, it MUST be registered in the `synth` abject (`array` type) before using it.

## Properties for a `synth` definition

| Property | type | Description |
| -------- | ---- | ----------- |
| `name` | `string` | (Required) The name of the synth. |
| `type` | `'FM'|'AM'` | (Default: `'FM'`) The type of the synth. |
| `carrierType` | `'sine'|'square'|'sawtooth'|'triangle'` (Hz) | (Default: `'sine'`) The wave form type of the carrier. |
| `carrierPitch` | `number` (Hz) | (Default: `220`Hz) The pitch frequency of the carrier. |
| `carrierDetune` | `number` (Hz) | (Default: `0`) The amount of detuning the carrier. |
| `modulatorType` | `'sine'|'square'|'sawtooth'|'triangle'` (Hz) | (Default: `'sine'`) The wave form type of the modulator. |
| `modulatorPitch` | `number` (Hz) | (Default: `220` Hz) The pitch frequency of the modulator. |
| `modulatorVolume` | `number<0-1>` | (Default: 0.2) The gain of the modulator. |
| `modulation` | `number` | (Only for `FM` synth, default: `1`) It is multiplied to the `carrierPitch` to get `modulatorPitch`. |
| `harmonicity` | `number` | (Only for `AM` synth, default: `1`) it is harmonic of the modulator node. |
| `attackTime` | `number` (seconds) | (Default: `0` sec.) The length of attack (when the highest peek comes). |
| `releaseTime` | `number` (seconds) | (Default: `0` sec.) The length of attack (how long it takes to ). |

Note: define either `modulatorPitch` or `modulation`/`harmonicity` because they are relatively defined.
If both provided, `modulation` is ignored.

### Usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "synth": [
    {
      "name": "my_synth",
      "attackTime": 0.2
    },
  ],
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let tone = new Erie.SynthTone("my_synth");
tone.attackTime(0.2);
...
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>

## Properties

A synth tone can have the following properties
