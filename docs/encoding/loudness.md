---
layout: default
title: Loudness Channel
parent: Encoding
level: 1
order: 705
---

Note: Erie Web Player does not map the `loudness` to the "volume" of a sound (i.e., actual decibel value)
because doing so may override or conflict with the user's volume setting.
For instance, a user with low hearing may have to set their device volumen higher than others,
and a user with sensitive hearing may have set it lower than others.
Instead, Erie Web Player maps the `loudness` to the `gain` (or velocity) of a sound (i.e., how strong a sound is played).
This way can work with users' different volume settings (i.e., relative volume control).

Using the `loudness` is pretty straightforward.
The range of `loudness` is always betwene `0` and `1`, and values exceeding this range will be capped.


### `loudness` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "loudness": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [0, 1] // opational
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
stream.enc.loudness.field("Body Mass (g)", "quantitative");
stream.enc.loudness.scale("domain", [0, 7000]); // optinal
stream.enc.loudness.scale("range", [0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->