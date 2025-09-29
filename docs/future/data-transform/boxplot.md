---
layout: future
title: Boxplot
parent: Transform
level: 1
order: 407
version: future
---

A `boxplot` transform generates statistics for making a box plot. 

Invalid values are filtered out.

## `Quantile` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `boxplot` | `string` | A field to make a boxplot of |
| `extent` | `number | 'min-max'` | (Optional, default: `1.5`) Threshold for outliers. Data values outside of the IQR (Interquartile Range) multipled by `extent` will be indicated as outliers. If it is `min-max` then there will be no outlier computed. |
| `groupby` | `Array[String]` | Nominal fields to group the boxplots by. |

### Output field names

In addition to the field names specified for `boxplot` and in `groupby`, a `boxplot` transform produces a predetermined field names:

| Field | Type | Description |
| ----- | ---- | ----------- |
| `key` | `string` | The type of this record: `whisker_lower` (lower bound), `q1` (25% qunatile), `median`, `q3` (75% qunatile), `whisker_upper`(upper bound), `outlier`. |
| `role` | `'point'|'outlier'` | Whether it is `'outlier'` or not (`'point'`). |
| `order` | `number` | The order of values (in ascending order): lower outliers, lower bound, 25% quantile, median, 75% quantile, and upper outliers. |
| `group_name` | `string` | Idnetifier for groups. |

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "boxplot": "speed",
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
let boxplot = new Erie.Boxplot("speed", 1.5) // field, extent;
boxplot.groupby(["category"]);
stream.transform.add(boxplot);
...
{% endhighlight %}
</code-group>
</code-groups>
