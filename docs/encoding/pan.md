---
layout: default
title: Pan Channel
parent: Encoding
level: 1
order: 606
version: current
---

In Erie, the `pan` channel means streo panning (left and right spatial positioning).
A `pan` value is from `-1` (left) to `1` (right).
This channel works when there is a 2-channel stereo audio output device (e.g., earphones).
Earphones or headphones are highly recommended (at least for testing)
because otherwise it's harder to identify the spatial position using regular audio devices (e.g., your laptop speaker).
When no earphones or headphones are provided, panning is better identifed when a listener keeps some distance from the output audio device.

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
        "domain": [1900, 1950, 2000], // optional
        "range": [-1, 0, 1] // optional
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
stream.encoding.pan.field("year", "quantitative");
stream.encoding.pan.scale("domain", [1900, 1950, 2000]); // optional
stream.encoding.pan.scale("range", [-1, 0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->