---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Tap Channels (Speed and Count)
parent: Encoding
level: 1
order: 703
---

A tap channel sets the number of repeated tones (only works for `discrete` tones, and recommended using with `relative` `timing`).
Essentially, tap and `duration` works in a similar way (there is only a volume control that enables tapping).
That is, a tap channel encodes a data value in a different scale than that of the `time` channel.

There are two tap channels: `tapSpeed` and `tapCount`. 
Either one is allowed at a time
because they are later compiled to `tap` property in the resulting `AudioGraph`. 

Note: a tapping sound should remain at least 0.1 seconds, otherwise WebAudio API may fail to render the sound.

Note: a tapping sound consists of a sound and pause. The ration of sound to pause is 1:0.4.
(Future update will allow for customizing this rate.)

### `tapSpeed` usage pattern

The `tapSpeed` channel maps a data value to the number of tappings per second, given the same fixed time of duration (defined using `band`).
When the `polarity` of the scale is `positive`, a higher value is mapped to more tap counts, and a lower value to fewer tap counts, 
where tappings are distributed evenly for the same time duration.
For instance, when the `scale`'s `domain` is `[0, 1]` and `range` is `[0, 10]` for the `band` of `2` seconds,
then a value of `0.5` is mapped to `10` tappings played over `2` seconds.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "tapSpeed": {
      "field": "density",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 1], // optional
        "range": [0, 10],
        "band": 2 // unit: seconds
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
stream.enc.tapSpeed.field("density", "quantitative");
stream.enc.tapSpeed.scale("domain", [0, 1]); // optinal
stream.enc.tapSpeed.scale("range", [0, 10]); // optinal
stream.enc.tapSpeed.scale("band", 2); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->


### `tapCount` usage pattern

The `tapCount` channel directly maps a data value to the number of tappings with the same speed.
The length of each tapping sound is determined by `band`. 
For instance, when the `scale`'s `domain` is `[0, 5]` and `range` is `[0, 10]` for the `band` of `0.2` seconds,
then a value of `2.5` is mapped to `5` tapping sounds each of which lasts 0.2 seconds.




<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "tapCount": {
      "field": "",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 5], // optional
        "range": [0, 10],
        "band": 0.2 // unit: seconds
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
stream.enc.tapCount.field("density", "quantitative");
stream.enc.tapCount.scale("domain", [0, 5]); // optinal
stream.enc.tapCount.scale("range", [0, 10]); // optinal
stream.enc.tapCount.scale("band", 0.2); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->