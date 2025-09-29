---
layout: future
title: Quantile
parent: Transform
level: 1
order: 408
version: future
---

A `quantile` transform converts a field into quantiles.

## `Quantile` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `quantile` | `string` | A field to make quantile of |
| `n` | `number` | (Optional) The number of quantiles to produce (equi-steps) |
| `step` | `number` | (Optional, between 0 and 1, exclusive) Quantile intervals |
| `as` | `[string, string]` | (Optional) New field names for probabilites and quantiles, respectively |
| `groupby` | `Array[String]` | Nominal fields to group the quantiles by. |

If both `n` and `step` provided, then `n` is considered and `step` is ignored. 
`step` is recomputed to generate equi-step, integer number of quantiles. 

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "quantile": "speed",
      "step": 0.05,
      "groupby": [
        "category"
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
let quantile = new Erie.Quantile("speed", undefined, 0.05, ["p", "q"]) // field, n, step, as(prob, quantile);
quantile.groupby(["category"]);
stream.transform.add(quantile);
...
{% endhighlight %}
</code-group>
</code-groups>
