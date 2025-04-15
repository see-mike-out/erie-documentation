---
layout: future
title: Modulation and Harmonicity Channel
parent: Encoding
level: 1
order: 609
version: future
---

For a synth tone, the modulator's pitch can be adjusted using `modulation` index (FM/AM) or `harmonicity` (AM) channels.

The higher the `modulation` index is, the more warping the sound is.

On the other hand, be careful using the `harmonicity` channel because it is not linear.
An integer harmonicity value that provides a more harmonious sound than non-integer values.
If you are familiar with harmony theory, multiples of `1`, `2`, `1.5`, and `1.33` work better in this order
while multiples of `1.25`, `1.2` are not good.

Both values must be greater than 0.

## Modulation (index) for FM synth

The `modulation` channel for an FM synth tone encodes the modulation index defined as the ratio of modulator's amplitude to modulator's frequency.

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "synth": [
    {
      "name": "Fm1",
      "type": "FM",
      "modulatorVolume": 50,
      ...
    }
  ],
  "tone": {
    "type": "Fm1",
    "continued": false
  },
  "encoding" : {
    "modulation": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [0, 10]
      }
    }
  }
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...

let synth = new Erie.SynthTone("Fm1");
synth.modulatorVolume(50);
stream.synth.add(synth);
...
stream.tone.set(synth);
stream.encoding.modulation.field("Body Mass (g)", "quantitative");
stream.encoding.modulation.scale("domain", [0, 7000]); // optinal
stream.encoding.modulation.scale("range", [0, 10]);
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Modulation (index) for AM synth

The `modulation` channel for an AM tone encodes the modulation index defined as modulator's amplitude / carrier's amplitude.

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "synth": [
    {
      "name": "Am1",
      "type": "AM",
      ...
    }
  ],
  "tone": {
    "type": "Am1",
    "continued": false
  },
  "encoding" : {
    "modulation": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [10, 500]
      }
    }
  }
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...

let synth = new Erie.SynthTone("Am1");
synth.type("AM");
stream.synth.add(synth);
...
stream.tone.set(synth);
stream.encoding.modulation.field("Body Mass (g)", "quantitative");
stream.encoding.modulation.scale("domain", [0, 7000]); // optinal
stream.encoding.modulation.scale("range", [10, 500]);
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Harmonicity

The `harmonicity` channel encodes the harmoniy between the (pitch) frequencies of the carrier and moudlator.
Harmonicity of 1 equivalents to one octave scale.
This only available for an AM synth.

### `pitch` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "synth": [
    {
      "name": "Am1",
      "type": "AM"
      ...
    }
  ],
  "tone": {
    "type": "Am1",
    "continued": false
  },
  "encoding" : {
    "harmonicity": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [0, 1]
      }
    }
  }
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...

let synth = new Erie.SynthTone("Am1");
synth.type('AM');
stream.synth.add(synth);
...
stream.tone.set(synth);
stream.encoding.harmonicity.field("Body Mass (g)", "quantitative");
stream.encoding.harmonicity.scale("domain", [0, 7000]); // optinal
stream.encoding.harmonicity.scale("range", [0, 1]);
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->