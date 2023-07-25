---
layout: default
title: Fold
parent: Transform
level: 1
order: 405
---

A `fold` transformation reshapes the data by unpivoting it.

Suppose a table with variables, `column1`, `column2`, `category1`, and `category2`.
Folding `column1` and `column2` by `category1` results in `key`, `value`, `category1`, and `category2` like below.

##### Before

| column1 | column2 | category1 | category2 |
| ------- | ------- | --------- | --------- |
| 1 | 2 | 'A' | 'a' |
| 3 | 5 | 'B' | 'b' |
| 4 | 6 | 'C' | 'c' |

##### After

| key | value | category1 | category2 |
| --- | ----- | --------- | --------- |
| column1 | 1 | 'A' | 'a' |
| column2 | 2 | 'A' | 'a' |
| column1 | 3 | 'B' | 'b' |
| column2 | 5 | 'B' | 'b' |
| column1 | 4 | 'C' | 'c' |
| column2 | 6 | 'C' | 'c' |

## `Fold` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `fold` | `string[]` | (Required) An array of field names to fold. |
| `by` | `string` | (Required) A nominal field to group by. |
| `exclude` | `boolean` | (Optional, default: `false`) Whether to drop other fields (not specified by `fold` and `by`). |
| `as` | `string[length=2]` | (Optional, default: `['key', 'value']`) New field names for folded variables. |

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "fold": [
        "column1", "column2"
      ],
      "by": "category1",
      "exclude": true,
      "as": [
        "measure", "value"
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
let fold = new Erie.Fold(["column1", "column2"], "category1"); // filter expression
// alt) let fold = new Erie.Fold();
//      fold.Fold("datum.cost > 30");
//      fold.by("category1");
fold.exclude(true);
fold.as(["measure", "value"]);
stream.transform.add(fold);
...
{% endhighlight %}
</code-group>
</code-groups>

### Extended pattern with a `repeat` channel

Using `fold` with a `repeat` channel enables expressing intervals repeated over a field.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "aggregate": [
        {
          "op": "mean",
          "field": "Miles_per_Gallon",
          "as": "Miles_per_Gallon_mean"
        },
        {
          "op": "stdevp",
          "field": "Miles_per_Gallon",
          "as": "Miles_per_Gallon_stdevp"
        }
      ],
      "groupby": [
        "Origin"
      ]
    },
    {
      "calculate": "datum.Miles_per_Gallon_mean - datum.Miles_per_Gallon_stdevp",
      "as": "Miles_per_Gallon_lower"
    },
    {
      "calculate": "datum.Miles_per_Gallon_mean + datum.Miles_per_Gallon_stdevp",
      "as": "Miles_per_Gallon_upper"
    },
    {
      "fold": [
        "Miles_per_Gallon_lower",
        "Miles_per_Gallon_mean",
        "Miles_per_Gallon_upper"
      ],
      "by": "Origin",
      "exclude": true,
      "as": [
        "measure",
        "statistics"
      ]
    }
  ],
  ...
  "encoding": {
    "time": {
      "field": "measure",
      "type": "nominal",
      "scale": {
        "band": 0.5,
        "order": [
          "Miles_per_Gallon_lower",
          "Miles_per_Gallon_mean",
          "Miles_per_Gallon_upper"
        ]
      }
    },
    ...
    "repeat": {
      "field": "Origin",
      ...
    }
    ...
  },
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let aggregate = new Erie.Aggregate();
aggregate.add("mean", "Miles_per_Gallon", "Miles_per_Gallon_mean");
aggregate.add("stdevp", "Miles_per_Gallon", "Miles_per_Gallon_stdevp");
aggregate.groupby(["Origin"]);
stream.transform.add(aggregate);

let calc1 = new Erie.Calculate("datum.Miles_per_Gallon_mean - datum.Miles_per_Gallon_stdevp")
calc1.as("Miles_per_Gallon_lower");
stream.transform.add(calc1);

let calc2 = new Erie.Calculate("datum.Miles_per_Gallon_mean + datum.Miles_per_Gallon_stdevp")
calc2.as("Miles_per_Gallon_upper");
stream.transform.add(calc2);

let fold = new Erie.Fold([
  "Miles_per_Gallon_lower",
  "Miles_per_Gallon_mean",
  "Miles_per_Gallon_upper"
], "Origin");
fold.exclude(true);
fold.as(["measure", "statistics"]);
stream.transform.add(fold);
...

stream.enc.time.field("measure", "nominal");
stream.enc.time.scale("timing", "relative");
stream.enc.time.scale("band", 0.5);
stream.enc.time.scale("order", [
  "Miles_per_Gallon_lower",
  "Miles_per_Gallon_mean",
  "Miles_per_Gallon_upper"
]);
...
stream.enc.repeat.field("Origin");
...
{% endhighlight %}
</code-group>
</code-groups>
