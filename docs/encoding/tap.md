---
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
because they are later compiled to `tap` property in the resulting `AudioGraph` object.

#### Note for precision
A tapping sound is controlled by a `GainNode` (i.e., controlling the volume)
because if making it multiple oscillators, then it can cause higher overload to certain environments (further terminating browsers due to that overlad) and make it almost impossible to schedule overlaid streams. 
To prevent unexpected browser overload, thus, each tapping sound and following pause's duration has limited precision upto 2 decimal points. 
Plus, Erie adds a bumper pause of 0.1 seconds because it may abruptly end without fully palying the tappings. 
This may result in a situation where your intended `band` for `tapSpeed` (i.e., the length of each tap) is 2 seconds, but resulting sound may be 2.02 + 0.1 (bumper pause) seconds (i.e., 2.12 seconds).

### `tapSpeed` usage pattern

The `tapSpeed` channel maps a data value to the number of tappings per second, given the same fixed duration of time (defined using `band`).
When the `polarity` of the scale is `positive`, a higher value is mapped to more tap counts, and a lower value to fewer tap counts,
where tappings are distributed evenly for the same time duration.
For instance, when the `scale`'s `domain` is `[0, 1]` and `range` is `[0, 10]` for the `band` of `2` seconds,
then a value of `0.5` is mapped to `10` tappings played over `2` seconds.
For the `tapSpeed` channel, the `pauseRate` property in `scale` can adjust the ratio between the tapping sound and the pause following it.
When there is only one tapping sound (e.g., the output `tapSpeed` value is 0.5 for the `band` of 2), then the sound is played as determined by `singleTappingPosition`.
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
For the `tapCount` channel, the `pauseRate` property in `scale` can adjust the ratio between the tapping sound and the pause following it.
Alternatively, the `pauseLength` property in `scale` makes it possible to directly set the pause length (unit: seconds).

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