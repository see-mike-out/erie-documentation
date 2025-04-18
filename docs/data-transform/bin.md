---
layout: default
title: Bin
parent: Transform
level: 1
order: 402
version: current
---


A `bin` transformation discretizes a variable into multiple bin buckets.
This creates two new fields for the bin bucket's starting point and ending point.

## `Bin` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `bin` | `String` | A variable to bin |
| `as` | `String` | (Optional) A new field name for the starting point of each bin bucket; if not provided, then it is `{bin field name}__bin`. |
| `end` | `String` | (Optional) A new field name for the ending point of each bin bucket; if not provided, then it is `{bin field name}__bin_end`. |
| `nice` | `Boolean` | (Default: `true`) Whether to use "nicely" defined bin buckets. |
| `maxbins` | `Number` | (Default: `10`) The maximum number of bin buckets (can be ignored). |
| `step` | `Number` | (Optional) The size of each bin bucket (can be adjusted). |
| `exact` | `Array[Number]` | (Optional) The definition of bin buckets. If provided, `nice`, `maxbins`, and `step` are ignored. |

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "bin": "cost",
      "as": "cost_bin_start",
      "end": "cost_bin_end",
      "nice": true, // use either one of the below
      "maxbins": 6,
      "exact": [0, 5, 10, 20, 50],
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
let bin = new Erie.Bin("cost"); // field name
bin.as("cost_bin_start", "cost_bin_end");
bin.nice(true); // use either one of the below
bin.maxbins(6);
bin.exact([0, 5, 10, 20, 50]);
stream.transform.add(bin);
...
{% endhighlight %}
</code-group>
</code-groups>
