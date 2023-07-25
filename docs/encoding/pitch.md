---
layout: default
title: Pitch Channel
parent: Encoding
level: 1
order: 704
---

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
stream.enc.pitch.scale("domain", [0, 7000]); // optinal
stream.enc.pitch.scale("range", ['C3', 'C5']); // optional
stream.enc.pitch.scale("range", [130.812, 523.251] ); // optional, equivalent (unit: `Hz`)
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->