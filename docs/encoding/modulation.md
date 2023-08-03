---
layout: default
title: Modulation and Harmonicity Channel
parent: Encoding
level: 1
order: 609
---

For a synth tone, the modulator's pitch can be adjusted using `modulation` (FM) or `harmonicity` (AM) channels.

Be careful using modulation and harmonicity channels because they give a sense of pleasantness of a sound,
but the scale is not linear, rather periodic because they are reified using sine/cosine curves.

Both values must be greater than 0.

## Modulation (index)

The `modulation` channel encodes the modulation index (= modulator's frequency / modulator's amplitude).

### `pitch` usage pattern

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
      "modulatorVolume": 1000,
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
tone.modulatorVolume(1000);
...
stream.tone.set(synth);
stream.enc.modulation.field("Body Mass (g)", "quantitative");
stream.enc.modulation.scale("domain", [0, 7000]); // optinal
stream.enc.modulation.scale("range", [0, 10]);
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Harmonicity

The `harmonicity` channel encodes the harmoniy between the (pitch) frequencies of the carrier and moudlator.
Harmonicity of 1 equivalents to one octave scale.

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
synth.type('AM')
...
stream.tone.set(synth);
stream.enc.harmonicity.field("Body Mass (g)", "quantitative");
stream.enc.harmonicity.scale("domain", [0, 7000]); // optinal
stream.enc.harmonicity.scale("range", [0, 1]);
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->