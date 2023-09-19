---
layout: default
title: Time Channel
parent: Encoding
level: 1
order: 601
---

In data sonification, time is a very important channel to avoid chaos.
There are three types of timing in terms of when a tone is played: absolute, relative, and simultaneous.
Absolute timing means that the time channel encodes a certain quantitative or temporal data variable,
so that a time position when a tone is played at corresponds to a quantity or date-time.
In contrast, relatively timed sonification plays one tone after another,
more applicable for a nomoinal or ordinal variable.
Simultaneously-timed sounds have 0 as their start time so that they are played at the same time.
Erie supports these timings using a single `time` channel.

Time channel plays another role, duration—how long a tone is played.
There are two types of duration: one along the same scale with the `time` channel and one that maps a different scale.
For the former, use `time2` channel; and for the latter, use `duration` or `tap`-related channel.
*Note: `duration`, `tapCount`, and `tapSpeed` only works for discrete tones (see the `tone` documentation).*

This documents introduces patterns for using the `time` channel.
The default unit for time-related channels is "second" unless specified otherwise.

## Absolute timing with fixed duration: `time`

The baseline pattern for absolute timing with fixed duration is using `field` property.
With the `scale` property, it is possible to set the total `length` of the sonification stream, as a shortcut to the `range` property.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "time": {
      "field": "Miles_per_Gallon",
      "type": "quantitative",
      "scale": {
        "length": 5, // unit: seconds
        "domain": [0, 5] // equivalent with the above line
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
stream.encoding.time.field("Miles_per_Gallon", "quantitative");
stream.encoding.time.field("Miles_per_Gallon"); // this and the below lines are equivalent to the above line
stream.encoding.time.type("quantitative");
stream.encoding.time.scale("length", 5); // unit: seconds
stream.encoding.time.scale("domain", [0, 5]);// equivalent with the above line
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Absolute timing with varied duration along the same scale: `time` and `time2`

When the duration of a tone is determined by another field that uses the same scale of the `time` channel's field,
one can use `time2` channel (similar to `x2` and `y2` channels in Vega-Lite).
No `scale` detail is available for this channel.
If the `time` channel includes `bin` then `time2` is not available because bin end point is encoded using the time duration.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "transform" : [{
    "aggregate": [
      { "op": "stdevp", "field": "Miles_per_Gallon", "as": "Miles_per_Gallon_stdevp" },
      { "op": "mean", "field": "Miles_per_Gallon", "as": "Miles_per_Gallon_mean" }
    ]
  }, {
    "calculate": "datum.Miles_per_Gallon_mean - datum.Miles_per_Gallon_stdevp", "as": "Miles_per_Gallon_stdevp_lower"
  }, {
    "calculate": "datum.Miles_per_Gallon_mean + datum.Miles_per_Gallon_stdevp", "as": "Miles_per_Gallon_stdevp_upper"
  }],
  ...
  "encoding" : {
    "time": {
      "field": "Miles_per_Gallon_stdevp_lower",
      "type": "quantitative",
      "scale": {
        "length": 5, // unit: seconds
      }
    },
    "time2": {
      "field": "Miles_per_Gallon_stdevp_upper",
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
let agg = new Erie.Aggregate()
agg.add("stdevp", "Miles_per_Gallon", "Miles_per_Gallon_stdevp");
agg.add("mean", "Miles_per_Gallon", "Miles_per_Gallon_mean")
stream.transform.add(agg);
let calc1 = new Erie.Calculate("datum.Miles_per_Gallon_mean - datum.Miles_per_Gallon_stdevp", "Miles_per_Gallon_stdevp_lower");
let calc2 = new Erie.Calculate("datum.Miles_per_Gallon_mean + datum.Miles_per_Gallon_stdevp", "Miles_per_Gallon_stdevp_upper")
stream.transform.add(calc1);
stream.transform.add(calc2);
...
stream.encoding.time.field("Miles_per_Gallon_stdevp_lower", "quantitative");
stream.encoding.time.scale("length", 5);
stream.encoding.time2.field("Miles_per_Gallon_stdevp_upper");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Relative timing with fixed duration: `time`

For relative timing, such as when sonifying the mean value of a quantitative field over a nominal/ordinal field,
set the `timing` property of the `scale` to `'relative'`, as shown below.
One can specify the length of each tone (if not specified by `duration` field) using the `band` property.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "time": {
      "field": "Origin",
      "type": "nominal",
      "scale": {
        "timing": "relative",
        "band": 0.5 // unit: seconds
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
stream.encoding.time.field("Origin", "nominal");
stream.encoding.time.scale("timing", "relative");
stream.encoding.time.scale("band", 0.5); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Simiultaneous timing

The `simultaneous` timing makes all the tones played at the same time (each tone's start time is 0).
It is recommended to use a separate `duration` channel with a nominal channel.
In this case, speech channels (`speechBefore`, `speechAfter`) are not supported (ignored).

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "time": {
      "field": "Origin",
      "type": "nominal",
      "scale": {
        "timing": "simultaneous"
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
stream.encoding.time.field("Origin", "nominal");
stream.encoding.time.scale("timing", "simultaneous");
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->

## Use of a `tick`

As an auditory axis, it's possible to use a `tick`.

It is defined using the following properties

| Property | type | Description |
| -------- | ---- | ----------- |
| `name` | `String` | (Optional) The name of the tick. |
| `interval` | `Number` (unit: seconds/beat) | (Default: `0.5`-seconds/`2`-beats) the interval between tick sounds. |
| `band` | `Number` (unit: seconds/beat) | (Default: `0.1`-seconds/`0.5`-beats) the length of each tick sound. |
| `playAtTime0` | `Boolean` | (Default: `true`) whether to play a tick sound at the beginnig of a stream. |
| `oscType` | `'sine'|'square'|'sawtooth'|'triangle'` | (Default: `'sine'`) the type of an oscillator. See [here](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode/type) for details. |
| `pitch` | `Number` | (Default: `150`) the pitch frequency of the tick sound. |
| `loudness` | `Number[0-1]` | (Default: `0.4`) the relative loudness of the tick sound. |

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "time": {
      "field": "Origin",
      "type": "nominal",
      "tick": {
        "name": "default_tick", // optional
        "interval": 0.5, // unit: seconds
        "playAtplayAtTime0": true, // default
        "oscType": "sine", // default
        "pitch": 150, // default
        "loudness": 0.4 // default
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
stream.encoding.time.tick(true); // set tick
stream.encoding.time.tick("name", "default_tick"); // optional
stream.encoding.time.tick("interval", 0.5) // unit: seconds
stream.encoding.time.tick("playAtplayAtTime0", true) // default
stream.encoding.time.tick("oscType", "sine") // default
stream.encoding.time.tick("pitch", 150) // default
stream.encoding.time.tick("loudness", 0.4) // default

// alternatively
let tick = new Erie.Tick("default_tick") // name, optional argument
tick.interval(0.5); // unit: seconds
tick.playAtplayAtTime0(true); // default
tick.oscType("sine"); // default
tick.pitch(150); // default
tick.loudness(0.4); // default
stream.encoding.time.tick(tick);

{% endhighlight %}
</code-group>
</code-groups>
