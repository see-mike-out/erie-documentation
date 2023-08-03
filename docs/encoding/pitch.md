---
layout: default
title: Pitch and Detune Channel
parent: Encoding
level: 1
order: 604
---

## Pitch

The `pitch` channel maps a data variable to a perceived frequency (high and low).
Note that a higher frequency value is often perceived as a higher note, but the pitch itself is not the only determinant of a frequency.
A `pitch` value can be written as a frequency or a note.
It is possible round to a note by setting `roundToNote` as `true` (default is `false`).
For example, if a mapped pitch value is 130.6Hz, then the pitch sound is rounded to a closest note C3 (130.812Hz).

### `pitch` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "pitch": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "roundToNote": true, // default: `false`
      "scale": {
        "doamin": [0, 7000], // optional
        "range": ['C3', 'C5'] // optional
        "range": [130.812, 523.251] // optional, equivalent (unit: `Hz`)
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
stream.enc.pitch.field("Body Mass (g)", "quantitative");
stream.enc.pitch.roundToNote(true); // optinal
stream.enc.pitch.scale("domain", [0, 7000]); // optional
stream.enc.pitch.scale("range", ['C3', 'C5']); // optional
stream.enc.pitch.scale("range", [130.812, 523.251] ); // optional, equivalent (unit: `Hz`)
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Detune

For noise tones, `pitch` can't work because of the underlying structure (i.e., there is no absolute baseline pitch).
Instead, we can use `detune` channel (it only works for noise tones).
Detune can range from `-1200` to `1200`, where each `100` is a distance between two notes.

### `detune` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "tone": {
    "type": "whiteNoise"
  },
  "encoding" : {
    "detune": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [0, 1200] // optional; this way each 1000 maps to one-note scaling
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
stream.tone.set(new Tone('whiteNoise'))
stream.enc.detune.field("Body Mass (g)", "quantitative");
stream.enc.detune.scale("domain", [0, 7000]); // optional
stream.enc.detune.scale("range", [0, 1200]); // optional; this way each 1000 maps to one-note scaling
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->