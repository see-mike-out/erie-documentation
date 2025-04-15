---
layout: future
title: Data
level: 0
order: 300
version: future
---

Erie supports two methods for providing the data: `values` and `url`.

For static streams:

| Property | type | Description |
| -------- | ---- | ----------- |
| `values` | `Datum[]` | A tidy data. |
| `url` | `UrlString` | A URL to a dataset. It is retrieved using Fetch API. |
| `name` | `String` | The name of a dataset, which is defined in the `datasets` object. |

For streaming streams:

| Property | type | Description |
| -------- | ---- | ----------- |
| `stream` | `true` | (Required for streaming streams) If set, the output will be a `StreamingStream` object, and the above properties will be ignored. |
| `test` | `DataSpec` | (Optional) Either `{ values: [ ... ] }` or `{ url: "..." }` |

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
stream.data.set("values", data);
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
stream.data.set("url", "data/penguins.json");
...
{% endhighlight %}
</code-group>
</code-groups>
