---
layout: future
title: Calculate
parent: Transform
level: 1
order: 404
version: future
---

A `calculate` transformation generates a new column by computing some operations.

## `Calculate` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `calculate` | `ExprString` | A filter expression. |
| `as` | `String` | (Required) A new field name. |
| `groupby` | `Array[String]` | (Optional) Nominal fields to group elements by. |

### Caculate expression

Use `datum` to refer to each row item.
To access a field (say, `cost`), you can use `datum.cost`.
Filter expressions allow basic JavaScript expressions.

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    {
      "calculate": "datum.cost + datum.expense",
      "as": "net_cost",
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
let calc = new Erie.Calculate("datum.cost + datum.expense", "net_cost"); // filter expression and new field name
// alt) let calc = new Erie.Calculate();
//      calc.calculate("datum.cost + datum.expense");
//      calc.as("net_cost");
stream.transform.add(calc);
...
{% endhighlight %}
</code-group>
</code-groups>
