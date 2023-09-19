---
layout: default
title: Tap Channels (Speed and Count)
parent: Encoding
level: 1
order: 603
---

A tap channel sets the number of repeated tones (only works for `discrete` tones, and recommended using with `relative` or `simultaneous` `timing`).
Essentially, tap and `duration` works in a similar way.
That is, a tap channel encodes a data value in a different scale from that of the `time` channel.

There are two tap channels: `tapSpeed` and `tapCount`.

### `tapSpeed` usage pattern

The `tapSpeed` channel maps a data value to the number of tappings per second, given the same fixed duration of time (defined using `band`).
When the `polarity` of the scale is `positive`, a higher value is mapped to more tap counts, and a lower value to fewer tap counts,
where tappings are distributed evenly for the same time duration.
For instance, when the `scale`'s `domain` is `[0, 1]` and `range` is `[0, 5]` for the `band` of `2` seconds,
then a value of `0.5` is mapped to `5` tappings played over `2` seconds.
For the `tapSpeed` channel, the `pauseRate` property in `scale` can adjust the ratio between the tapping sound and the following pause.
When there is only one tapping sound (e.g., the output `tapSpeed` value is 0.1 for the `band` of 2), then the sound is played as determined by `singleTappingPosition`.
The default `singleTappingPosition` value is `'middle'` (i.e., pause > tap > pause).
If it is `'start'` then tap > pause; if it is `'end'` then pause > tap.
To cap the length of tapping sound, one can use `maxTappingLength` (unit: seconds; default: 0.3).

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
        "band": 2, // unit: seconds
        "singleTappingPosition": "start" // or "middle" / "end"
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
stream.encoding.tapSpeed.field("density", "quantitative");
stream.encoding.tapSpeed.scale("domain", [0, 1]); // optional
stream.encoding.tapSpeed.scale("range", [0, 10]); // optional
stream.encoding.tapSpeed.scale("band", 2); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

### `tapCount` usage pattern

The `tapCount` channel directly maps a data value to the number of tap sounds with the same speed.
The length of each tap sound is determined by `band`.
For instance, when the `scale`'s `domain` is `[0, 5]` and `range` is `[0, 10]` for the `band` of `0.2` seconds,
then a value of `2.5` is mapped to `5` tap sounds each of which lasts 0.2 seconds.
For the `tapCount` channel, the `pauseRate` property in `scale` can adjust the ratio between the tapping sound and the pause following it.
Alternatively, the `pauseLength` property in `scale` makes it possible to directly set the pause length (default unit: seconds).

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "tapCount": {
      "field": "length",
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
stream.encoding.tapCount.field("length", "quantitative");
stream.encoding.tapCount.scale("domain", [0, 5]); // optional
stream.encoding.tapCount.scale("range", [0, 10]); // optional
stream.encoding.tapCount.scale("band", 0.2); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

### `tapCount` + `tapSpeed`

You can use `tapCount` and `tapSpeed` channels together. In this case, the `band` of `tapSpeed` (i.e., the duration), the `pause` of `tapCount`, and the `singleTappingPosition` of `tapSpeed` are all ignored.
It is basically a `tapCount` channel with varying speeds.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "tapCount": {
      "field": "length",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7], // optional
        "range": [0, 7], // optional
        "band": 0.2 // unit: seconds
      }
    },
     "tapSpeed": {
      "field": "sparsity",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 1], // optional
        "range": [0, 7], // optional
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
stream.encoding.tapCount.field("length", "quantitative");
stream.encoding.tapCount.scale("domain", [0, 7]); // optional
stream.encoding.tapCount.scale("range", [0, 7]); // optional
stream.encoding.tapCount.scale("band", 0.2); // unit: seconds
stream.encoding.tapSpeed.field("sparsity", "quantitative");
stream.encoding.tapSpeed.scale("domain", [0, 1]); // optional
stream.encoding.tapSpeed.scale("range", [0, 7]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

### Notes

Excessive tappings (e.g., 30 taps within 0.5 seconds) may cause browser crash due to memory shortage (it is not really a good sonification design as well).

Each tapping sound and following pause's duration has limited precision upto 2 decimal points.
This may result in a situation where your intended `band` for `tapSpeed` (i.e., the length of each tap) is 2 seconds, but resulting sound may be 2.02.

Given that there is no concept of 0.5 tap sound, so the final tap counts are all rounded.
