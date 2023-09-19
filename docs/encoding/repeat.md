---
layout: default
title: Repeat Channel
parent: Encoding
level: 1
order: 610
---

The `repeat` channel is a data-driven composition method that sequences and/or overlays streams over one or more nominal fields.
If used, then tones are grouped and played separately.
It is possible to specify the order to be played using either `order` or `sort` property (not both).

### A basic `repeat` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "repeat": {
      "field": "region",
      "type": "nominal",
      "speech": true, // before playing each stream, a speech sound for the correspondig value is played.
      "by": "sequence", // the streams are sequenced
      "scale": {
        "order": ["Asia", "Europe", "North America"], // optional
        "sort": "ascending" // optional (not together with `order`)
        "description": "skip" // then a scale description is omitted.
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
stream.encoding.repeat.field("region", "nominal");
stream.encoding.repeat.speech(true); // before playing each stream, a speech sound for the correspondig value is played.
stream.encoding.repeat.by("sequence"); // the streams are sequenced
stream.encoding.repeat.scale("order", ["Asia", "Europe", "North America"]); // optional
stream.encoding.repeat.scale("sort", "ascending"); // optional (not together with `order`)
stream.encoding.repeat.scale("description", "skip");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- example -->

### Multi-field `repeat` channel

To enable nested repetition, provide `field`, `by`, and `scale.order` using arrays.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "repeat": {
      "field": ["region", "category"],
      "type": "nominal",
      "speech": true,
      "by": ["sequence", "overlay"], // streams are sequenced for "region" and overlaid for "category"
      "scale": {
        "order": [
          ["Asia", "Europe", "North America"],
          ["Customer", "Business", "Non-profit"]
        ], // Asia-Customer > Asia-Business > Asia-Non-profit > ... > North America-Non-profit
        "description": "skip"
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
stream.encoding.repeat.field(["region", "category"], "nominal");
stream.encoding.repeat.by(["sequence", "overlay"]); // streams are sequenced for "region" and overlaid for "category"
stream.encoding.repeat.speech(true); // before playing each stream, a speech sound for the correspondig value is played.
stream.encoding.repeat.scale("order", [["Asia", "Europe", "North America"], ["Customer", "Business", "Non-profit"]]); // Asia-Customer > Asia-Business > Asia-Non-profit > ... > North America-Non-profit
stream.encoding.repeat.scale("description", "skip");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- example -->