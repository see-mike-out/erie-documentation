---
layout: default
title: Transform
level: 0
order: 400
---

Erie supports several basic data transformation operations.
The transformations are exectued in the specified order.
Transformations that change the shape of the data set may drop non-computed fields/columns.

## Supoorted transformation operations

| Operation | Description | Shape changes |
| --------- | ----------- | ------------- |
| `aggregate` | One or more data aggregation operation. | Yes-Column/Row |
| `bin` | Binning a field | No |
| `filter` | Filter in data points | No |
| `calculate` | Compute a new field | No |
| `fold` | Convert columns to values | Yes-Column/Row |
| `density` | Compute a kernel density | Yes-Column/Row |

### API usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "transform" : [
    { ... },
    { ... },
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
stream.transform.add({ ... });
stream.transform.add({ ... });
...
{% endhighlight %}
</code-group>
</code-groups>


For JavaScript API,
you can also use `stream.transform.remove(index)` to remove a transform item by index.