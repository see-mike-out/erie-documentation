---
layout: default
title: Data
level: 0
order: 300
---

Erie supports two methods for providing the data: `values` and `url`.

| Property | type | Description |
| -------- | ---- | ----------- |
| `values` | `object[]` | A tidy data. |
| `url` | `urlString` | A URL to a dataset. It is retrieved using Fetch API. |

Specifying named datasets is under plan.

### Specifying data using `values`

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "data" : {
    "values": [
      {
        "category": "A",
        "sparsity": 0.5
      },
      {
        "category": "B",
        "sparsity": 0.1
      },
      {
        "category": "C",
        "sparsity": 0.9
      },
      {
        "category": "D",
        "sparsity": 1
      },
      {
        "category": "E",
        "sparsity": 0.75
      }
    ]
  }
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let data = [
  {
    "category": "A",
    "sparsity": 0.5
  },
  {
    "category": "B",
    "sparsity": 0.1
  },
  {
    "category": "C",
    "sparsity": 0.9
  },
  {
    "category": "D",
    "sparsity": 1
  },
  {
    "category": "E",
    "sparsity": 0.75
  }
];
let stream = new Erie.Stream();
...
stream.data.set(data);
...
{% endhighlight %}
</code-group>
</code-groups>

### Specifying data using `url`

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "data" : {
    "url": "data/penguins.json"
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
stream.data.set("data/penguins.json");
...
{% endhighlight %}
</code-group>
</code-groups>
