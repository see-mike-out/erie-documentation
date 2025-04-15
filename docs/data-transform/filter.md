---
layout: default
title: Filter
parent: Transform
level: 1
order: 403
version: current
---

A `filter` transformation filters "IN" rows that meet a provided condition.

## `Filter` properties

| Property | Type | Description |
| -------- | ---- | ----------- |
| `filter` | `ExprString` | A filter expression. |

### Filter expression

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
      "filter": "datum.cost > 30",
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
let filter = new Erie.Filter("datum.cost > 30"); // filter expression
// alt) let filter = new Erie.Filter(); filter.filter("datum.cost > 30");
stream.transform.add(filter);
...
{% endhighlight %}
</code-group>
</code-groups>
