---
layout: default
title: Aggregate
parent: Transform
level: 1
order: 401
---

An `aggregate` transformation offers aggregating one or more fields.
Because `aggregate` drops columns that are not specified by `aggregate` or `groupby`,
make sure that include all fields needed to be aggregated.

## `Aggregate` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `aggregate` | `Array[Object]` | One or more data aggregation operation. |
| `groupby` | `Array[String]` | Nominal fields to group the aggregates by. |

### `aggregate` operation definition

| Property | Type | Description |
| -------- | ---- | ----------- |
| `op` | `String` | An aggregate operation. |
| `field` | `String|Array[String]` | A field(s) to aggregate. Some operations require two fields. |
| `p` | `Number[0-1]` | A quantile threshould. |
| `as` | `String` | (Optional) A new field name. |

### Supported aggregate operations

For detailed documentation, refer to [this page (Arquero)](https://uwdata.github.io/arquero/api/op).

#### Zero-field operation

No `field` property is required. If provided, it will be ignored.

- `count`: the count of elements

#### Single-field operation

The `field` should be a string for a single field.

- `valid`: the number of valid items in a variable
- `distinct`: the number of distinct items in a variable

- `mean`/`average`: the mean of a variable
- `mode`: the mode of a variable
- `median`: the median of a variable
- `quantile`: the `p` quantile value of a variable
- `stdev`: the standard deviation of a variable
- `stdevp`: the population standard deviation of a variable
- `variance`: the variance of a variable
- `variancep`: the population variance of a variable

- `sum`: the sum of a variable
- `product`: the product (multiplication) of a variable
- `max`: the maximum value of a variable
- `min`: the min value of a variable

#### Double-field operation

The `field` should be a string for a single field.

- `corr`: the correlation of two variables
- `covariance`: the covariance of two variables
- `covariancep`: the population covariance of two variables

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "aggregate": [
        { "op": "count", "as": "count" },
        { "op": "mean", "field": "cost", "as": "mean_cost" },
        { "op": "quantile", "field": "cost", "p": 0.7 "as": "top_30_cost" },
        { "op": "covariance", "field": ["cost", "expense"], "as": "cost_expense_covariance" }
      ],
      "groupby": [
        "category", "country"
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
let aggregate = new Erie.Aggregate();
aggregate.add("count", "count"); // op, as
aggregate.add("mean", "cost", "count");
aggregate.add("quantile", "cost", "top_30_cost", 0.7); // op, field, as, p
aggregate.add("covariance", ["cost", "expense"], "cost_expense_covariance");
aggregate.groupby(["category", "country"]);
stream.transform.add(aggregate);
...
{% endhighlight %}
</code-group>
</code-groups>
