---
layout: default
title: Duration Channel
parent: Encoding
level: 1
order: 602
---

The `duration` channel sets the length of each tone (only works for discrete tones)
that encodes a data value in a different scale than that of the `time` channel.
An example case is mapping a `year` field to the `time` channel and a `price` field to the `duration` channel.
The default unit is *seconds*.
The `duration` channel has a relatively simple usage pattern.

### `duration` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "duration": {
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
stream.encoding.time.field("Body Mass (g)", "quantitative");
stream.encoding.time.scale("domain", [0, 7000]); // optinal
stream.encoding.time.scale("range", [0, 1]); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->