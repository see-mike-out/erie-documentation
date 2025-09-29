---
layout: default
title: Loudness Channel
parent: Encoding
level: 1
order: 605
version: current
---

The `loudness` channel maps a data variable to how loud a sound is (or volume).
Erie.js compiles `loudness` as gain, instead of physical volume that is measured in decibel
because doing so may override or conflict with the user's volume setting.
For instance, a user with low hearing may have to set their device volumen higher than others,
and a user with sensitive hearing may have set it lower than others.
In addition, sampled audio has its own default volume, so controling them is nearly impossible.
Instead, Erie Web Player maps the `loudness` to the `gain` (or velocity) of a sound (i.e., how strong a sound is played; or the amplitude of a sound),
so that it works with users' different volume settings (i.e., relative volume control).
It's always the very listener who has the control to the actual volume.

The unit of `loudness`/gain is from `0` to infinity, where 0 means muted sound (the signal is flat) and 1 means the default volume.
However, if a loudness value is beyond 1 or 2, the sound might be too loud all at sudden, so we set the default loudness range to `[0, 1]`.

Using the `loudness` is pretty straightforward.

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
        "domain": [0, 7000], // optional
        "range": [0, 1] // optional
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
stream.encoding.loudness.field("Body Mass (g)", "quantitative");
stream.encoding.loudness.scale("domain", [0, 7000]); // optional
stream.encoding.loudness.scale("range", [0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->