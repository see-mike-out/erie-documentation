---
layout: future
title: Diffing
parent: Transform
level: 1
order: 409
version: future
---

A `diffing` transform computes the difference between consecutive data points.

## `Quantile` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `diffing` | `string[]` | Fields to compute diffings. Because this transformation reshape the data, provide all the fields to compute diffing together. |
| `carryOver` | `boolean` | (Optional, default: `true`, only for streaming specs) Whether to get the last value from the lastest instance in history. |
| `keepFirstAsZero` | `boolean` | (Optional, default: `true`) If add a zero value before the first datum. For streaming specs, if `carryOver` is set to `true` and there are historical data points, then this is ignored. |
| `as` | `string[]` | (Optional) New field names. The length must match that of `diffing`. |
| `groupby` | `Array[String]` | Nominal fields to group the diffed values by. |

### Behavior

Suppose we have a dataset with three fields: `category`, `speed`, and `distance`, and we compute `diffing` for `speed` and `distance`.

| category | speed | distance |
| -------- | ----- | -------- |
| car      | 10    | 300      |
| car      | 11    | 290      |
| bird     | 1     | 12       |
| bird     | 1.1   | 13       |

If `groupby` is not set, then it will result in:

| category | speed_diff | distance_diff |
| -------- | ---------- | ------------- |
| car      | 10         | 300           |
| car      | 1          | -10           |
| bird     | -10        | -278          |
| bird     | 0.1        | 1             |

If `groupby` is set to `['category]`, then it will result in:

| category | speed_diff | distance_diff |
| -------- | ---------- | ------------- |
| car      | 10         | 300           |
| car      | 1          | -10           |
| bird     | 1          | 12            |
| bird     | **0.1**    | **1**         |

As shown above, 'next' values are preserved for the non-diffed fields.

If `keepFirstAsZero` is set to `false` (with `groupby`), it will reslut in:

| category | speed_diff | distance_diff |
| -------- | ---------- | ------------- |
| car      | 1          | -10           |
| bird     | 0.1        | 1             |

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "diffing": ["speed", "distance"],
      "groupby" ["category"],
      "carryOver": true
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
let diffing = new Erie.Diffing(["speed", "distance"], true, true, ["speed_diff", "distance_diff"]) // fields, carryOver, keepFirstAsZero, new field names
diffing.groupby(['category']);
stream.transform.add(diffing);
...
{% endhighlight %}
</code-group>
</code-groups>
