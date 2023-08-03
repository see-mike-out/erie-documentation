---
layout: default
title: Pan Channel
parent: Encoding
level: 1
order: 606
---

In Erie, the `pan` channel means streo panning (left and right spatial positioning).
3D panning wiht X, Y, and Z positions in a plan for the future development.
A `pan` value is from `-1` (left) to `1` (right).
This channel works when there is a 2-channel stereo audio output device (e.g., earphones), 
and earphones or headphones are highly recommended (at least for testing) 
because otherwise it's harder to identify the spatial position using regular audio devices (e.g., your laptop speaker).

### `pan` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "pan": {
      "field": "year",
      "type": "quantitative",
      "scale": {
        "doamin": [1900, 1950, 2000], // optional
        "range": [-1, 0, 1] // opational
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
stream.enc.pan.field("year", "quantitative");
stream.enc.pan.scale("domain", [1900, 1950, 2000]); // optinal
stream.enc.pan.scale("range", [-1, 0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->