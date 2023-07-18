---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Time Channel
parent: Encoding
level: 1
order: 701
---

In data sonification, time is a very important channel to avoid chaos.
There are two types of timing in terms of when a tone is played: absolute and relative.
Absolute timing means that the time channel encodes a certain quantitative or temporal data variable,
so that a time position when a tone is played at corresponds to a quantity or date-time.
In contrast, relatively timed sonification plays one tone after another,
more applicable for a nomoinal or ordinal variable.
Erie supports both timings using a single `time` channel.

Time channel plays another role, duration—how long a tone is played.
There are two types of duration: one along the same scale with the `time` channel and one that maps a different scale.
For the former, use `time2` channel; and for the latter, use `duration` or `tap` channel.
*Note: `duration` and `tap` only works for non-continued tones (see the `tone` documentation).*

This documents introduces patterns for using the `time` channel.
The default unit for time-related channels is "second" unless specified otherwise.

## Absolute timing with fixed duration: `time`

The baseline pattern for absolute timing with fixed duration is using `field` attribute.
With the `scale` attribute, it is possible to set the total `length` of the sonification stream, as a shortcut to the `range` attribute.

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
      "sclae": {
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
stream.enc.time.field("Miles_per_Gallon", "quantitative");
stream.enc.time.field("Miles_per_Gallon"); // this and the below lines are equivalent to the above line
stream.enc.time.type("quantitative");
stream.enc.time.scale("length", 5); // unit: seconds
stream.enc.time.scale("domain", [0, 5]);// equivalent with the above line
...
{% endhighlight %}
</code-group>
</code-groups>

## Absolute timing with varied duration along the same scale: `time` and `time2`

When the duration of a tone is determined by another field that uses the same scale of the `time` channel's field,
one can use `time2` channel (similar to `x2` and `y2` channels in Vega-Lite).
No `scale` detail is available for this channel.
If the `time` channel includes `bin` then `time2` is not available because bin size is encoded using the time duration.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "transform" : [{
    "aggregate": "stdevp", "field": "Miles_per_Gallon", "as": "Miles_per_Gallon_stdevp"
  }, {
    "aggregate": "mean", "field": "Miles_per_Gallon", "as": "Miles_per_Gallon_mean"
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
      "sclae": {
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
stream.transform.add("aggregate", "stdevp", "Miles_per_Gallon", "Miles_per_Gallon_stdevp");
stream.transform.add("aggregate", "mean", "Miles_per_Gallon", "Miles_per_Gallon_mean");
stream.transform.add("calculate",  "datum.Miles_per_Gallon_mean - datum.Miles_per_Gallon_stdevp", "Miles_per_Gallon_stdevp_lower");
stream.transform.add("calculate",  "datum.Miles_per_Gallon_mean + datum.Miles_per_Gallon_stdevp", "Miles_per_Gallon_stdevp_upper");
...
stream.enc.time.field("Miles_per_Gallon_stdevp_lower", "quantitative);
stream.enc.time.scale("length", 5); 
stream.enc.time2.field("Miles_per_Gallon_stdevp_upper");
...
{% endhighlight %}
</code-group>
</code-groups>

## Relative timing with fixed duration: `time`

For relative timing, such as when sonifying the mean value of a quantitative field over a nominal/ordinal field,
use the `timing` attribute of the `scale` with the value of `relative`, as shown below.
One can specify the length of each tone (if not specified by `duration` field) using the `band` attribute.

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
      "sclae": {
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
stream.enc.time.field("Origin", "nominal");
stream.enc.time.scale("timing", "relative");
stream.enc.time.scale("band", 0.5); // unit: seconds
...
{% endhighlight %}
</code-group>
</code-groups>