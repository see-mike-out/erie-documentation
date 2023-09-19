---
layout: default
title: Periodic Wave
parent: Tone
level: 1
order: 501
---

It is possible to provide a periodic wave form (see [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicWave))
by providing a periodic wave object.
These instrument types suppport `continued` tones.

## 'wave' object properties

A top-level object `wave` is a list of objects with the following properties.

| Property | type | Description |
| -------- | ---- | ----------- |
| `name` | `String` | (Required) The name for indexing. |
| `real` | `Array[Number]` | (Required) Sine terms. |
| `image` | `Array[Number]` | (Required) Cosine terms. |
| `disableNormalization` | `Boolean` | (Defalut: `false`) Whether to normalize the wave—fix the maximum amplitude at 1 (see the MDN doc). |

### API Usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "wave": [{
    "name": "wave1",
    "real": [ ... ], // sine terms
    "imag": [ ... ] // cosine terms
  }]
  "tone": {
    "type": "wave1",
    ...
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
let wave = new Erie.Wave("wave1", periodicWave);
// alt1) let wave = new Erie.Wave("wave1"); wave.wave(periodicWave);
// alt2) let wave = new Erie.Wave("wave1"); wave.real(periodicWave.real); wave.imag(periodicWave.imag);

stream.wave.add(wave);
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>