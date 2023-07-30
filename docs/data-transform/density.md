---
layout: default
title: Density
parent: Transform
level: 1
order: 406
---

A `density` transformation converts a variable into an estimated kernel density using Vega's density transform (see [this documentation](https://vega.github.io/vega-lite/docs/density.html)), resulting in a density distribution

## `Density` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `density` | `string` | (Required) A variable name to estimate the density |
| `groupby` | `string[]` | (Optinal) The names of variables to group items by. |
| `cumulative` | `boolean` | (Optinal, default: `false`) Whether to compute cumulative density or probability density. |
| `counts` | `boolean` | (Optinal, default: `false`) Whether to compute probabilty density (sum to 1) or count-wise density (actual counts, not sum to 1). |
| `bandwidth` | `number` | (Optinal) The kernel's bandwidth. If not provided, it's automatically estimated |
| `extent` | `number[length=2]` | (Optinal) A `[min, max]` range for the kernel density estimation. |
| `minsteps` | `number` | (Optinal, default: `25`) The minimum number of sampled values. |
| `maxsteps` | `number` | (Optinal, default: `200`) The maximum number of sampled values. |
| `steps` | `number` | (Optinal) The exact number of sampled values |
| `as` | `string[length=2]` | (Optional, default: `['value', 'density']`) The new field names for the estimation. |

### Basic usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "density": "Body Mass (g)",
      "extent": [
        2500,
        6500
      ]
    },
  ]
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let density = new Erie.Density("Body Mass (g)"); // shortcut constructor
// alt) let density = new Erie.Density(); density.field("Body Mass (g)");
density.extent(2500, 6500);
// alt) density.extent([2500, 6500]);
stream.transform.add(density);
...
{% endhighlight %}
</code-group>
</code-groups>

### Advanced: `density` + `groupby` + `repeat`

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "density": "Body Mass (g)",
      "groupby": [
        "Species",
        "Island"
      ],
      "extent": [
        2500,
        6500
      ]
    },
  ],
  "tone": {
    "continued": true
  },
  "encoding": {
    "time": {
      "field": "value",
      "type": "quantitative",
      "scale": {
        "length": 3
      }
    },
    "pitch": {
      "field": "density",
      "type": "quantitative",
      "scale": {
        "polarity": "positive",
        "range": [
          0,
          700
        ]
      }
    },
    "repeat": {
      "field": [
        "Species",
        "Island"
      ],
      "type": "nominal",
      ...
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
let groupby = ["Species", "Island"];
let density = new Erie.Density("Body Mass (g)"); // no shortcut constructor
density.groupby(groupby);
density.extent(2500, 6500);
// alt) density.extent([2500, 6500]);
stream.transform.add(density);
...

let tone = new Erie.Tone("default", true);
stream.tone.set(tone);
...
stream.enc.time.field("value", "quantitative");
stream.enc.time.scale("length", 3);

stream.enc.pitch.field("density", "quantitative");
stream.enc.pitch.scale("polarity", "positive");
stream.enc.pitch.scale("range", [0, 700]);

stream.enc.repeat.field(groupby);
...
{% endhighlight %}
</code-group>
</code-groups>
