---
layout: default
title: Repeat Channel
parent: Encoding
level: 1
order: 709
---

The `repeat` channel is a way to make sequanced streams
that are repeated over one or more nominal fields.
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
stream.enc.speechAfter.field("region", "nominal");
stream.enc.speechAfter.speech(true); // before playing each stream, a speech sound for the correspondig value is played.
stream.enc.speechAfter.scale("order", ["Asia", "Europe", "North America"]); // optional
stream.enc.speechAfter.scale("sort", "ascending"); // optional (not together with `order`)
stream.enc.speechAfter.scale("description", "skip");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- example -->