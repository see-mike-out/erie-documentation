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
stream.enc.repeat.field("region", "nominal");
stream.enc.repeat.speech(true); // before playing each stream, a speech sound for the correspondig value is played.
stream.enc.repeat.scale("order", ["Asia", "Europe", "North America"]); // optional
stream.enc.repeat.scale("sort", "ascending"); // optional (not together with `order`)
stream.enc.repeat.scale("description", "skip");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- example -->


### Multi-field `repeat` channel

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
stream.enc.repeat.field(["region", "category"], "nominal");
stream.enc.repeat.speech(true); // before playing each stream, a speech sound for the correspondig value is played.
stream.enc.repeat.scale("order", [["Asia", "Europe", "North America"], ["Customer", "Business", "Non-profit"]]); // Asia-Customer > Asia-Business > Asia-Non-profit > ... > North America-Non-profit
stream.enc.repeat.scale("description", "skip");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- example -->