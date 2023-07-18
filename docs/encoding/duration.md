---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Duration Channel
parent: Encoding
level: 1
order: 702
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
stream.enc.time.field("Body Mass (g)", "quantitative");
stream.enc.time.scale("domain", [0, 7000]); // optinal
stream.enc.time.scale("range", [0, 1]); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>