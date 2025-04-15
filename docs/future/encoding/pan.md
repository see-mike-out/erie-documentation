---
layout: future
title: Pan Channel
parent: Encoding
level: 1
order: 606
version: future
---

Erie supports 3d panning in two ways: Cartesian and Angular coordinates.
Earphones or headphones are highly recommended (at least for testing)
because otherwise it's harder to identify the spatial position using regular audio devices (e.g., your laptop speaker).
When no earphones or headphones are provided, panning is better identifed when a listener keeps some distance from the output audio device.

## Stereo Panning

For typical stereo panning, use either `pan` or `panX` channel.
A `pan` value is from `-1` (left) to `1` (right).
This channel works when there is a 2-channel stereo audio output device (e.g., earphones).

### `pan` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "pan": {
      "field": "year",
      "type": "quantitative",
      "scale": {
        "doamin": [1900, 1950, 2000], // optional
        "range": [-1, 0, 1] // optional
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
stream.encoding.pan.field("year", "quantitative");
stream.encoding.pan.scale("domain", [1900, 1950, 2000]); // optinal
stream.encoding.pan.scale("range", [-1, 0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

## 3D Cartesian Panning

To use the Cartesian coordinates, use `panX` (left-right), `panY` (top-down), and `panZ` (front-back) channels.
Their ranges are all from `-1` to `1`.

### `panX`, `panY`, and `panZ` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "panX": {
      "field": "year",
      "type": "quantitative",
      "scale": {
        "doamin": [1900, 1950, 2000], // optional
        "range": [-1, 0, 1] // optional
      }
    },
    "panY": {
      "field": "revenue",
      "type": "quantitative",
      "scale": {
        "doamin": [-500, 0, 500], // optional
        "range": [-1, 0, 1] // optional
      }
    },
    "panZ": {
      "field": "location",
      "type": "ordinal",
      "scale": {
        "doamin": ['A', 'B', 'C'], // optional
        "range": [-1, 0, 1] // optional
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
stream.encoding.panX.field("year", "quantitative")
  .scale("domain", [1900, 1950, 2000]) // optinal
  .scale("range", [-1, 0, 1]); // optional
stream.encoding.panY.field("revenue", "quantitative")
  .scale("domain", [-500, 0, 500]) // optinal
  .scale("range", [-1, 0, 1]); // optional
stream.encoding.panZ.field("location", "ordinal")
  .scale("domain", ['A', 'B', 'C']) // optinal
  .scale("range", [-1, 0, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

## 3D Angular Panning

To use the Angular coordinates, use `panRadius` (distance), `panPolar` (angle from the *Z* axis), and `panAzimuth` (angle from the *X* axis) channels.
Raidus ranges from `0` to `1`, and `panPolar` and `panAzimuth` can take any number in degrees. If a `panPolar` value is 422, then it is as same as 62 (= 422 mod 360). This allows for cyclic data patterns.

### `panRadius`, `panPolar`, and `panAzimuth` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "panAzimuth": {
      "field": "year",
      "type": "quantitative",
      "scale": {
        "doamin": [1900, 2000], // optional
        "range": [0, 600] // optional (every 5 year = 30 deg)
      }
    },
    "panPolar": {
      "field": "revenue",
      "type": "quantitative",
      "scale": {
        "doamin": [-500, 0, 500], // optional
        "range": [-360, 0, 360] // optional
      }
    },
    "panRadius": {
      "field": "location",
      "type": "ordinal",
      "scale": {
        "doamin": ['A', 'B', 'C'], // optional
        "range": [0, 0.5, 1] // optional
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
stream.encoding.panAzimuth.field("year", "quantitative")
  .scale("domain", [1900, 2000]) // optinal
  .scale("range", [0, 600]); // optional (every 5 year = 30 deg)
stream.encoding.panPolar.field("revenue", "quantitative")
  .scale("domain", [-500, 0, 500]) // optinal
  .scale("range", [-360, 0, 360]); // optional
stream.encoding.panRadius.field("location", "ordinal")
  .scale("domain", ['A', 'B', 'C']) // optinal
  .scale("range", [0, 0.5, 1]); // optional
...
{% endhighlight %}
</code-group>
</code-groups>

## Conflict Resolution between Coordinates

If both Cartesian and Angular channels are provided, then the more saturated coordinates (i.e., more channels) will be used.
If they are equally saturated, then Cartesian channels have priorities.

## Custom Coordinates

Use `calculate` transform to have your own custom coordianates (e.g., along a 3D curve).

## Default 3D Panner Settings

Erie uses the following settings for a 3D panner.

| Properties | Values |
|------------|--------|
| `panningModel` | `equalPower` |
| `distanceModel` | `inverse` |
| `refDistance` | `1` |
| `maxDistance` | `10000` |
| `rolloffFactor` | `1` |
| `coneInnerAngle` | `360` |
| `coneOuterAngle` | `360` |
| `coneOuterGain` | `0` |

(Future support: customizing of 3D panner)
