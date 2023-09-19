---
layout: default
title: Datasets
level: 0
order: 301
---

The `datasets` object allows for registering datasets and used in streams.
It is useful when there are multiple streams in an Erie specification.

A `datasets` object is an array of `data` objects.
Each `data` objects has the following properties.

| Property | type | Description |
| -------- | ---- | ----------- |
| `name` | `String` | (Required) The name of a dataset. This is the index for the specified data. |
| `values` | `Array[Object]` | A tidy data. |
| `url` | `UrlString` | A URL to a dataset. It is retrieved using Fetch API. |

### Usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "datasets": [{
    "name": "data1",
    "url": "...path1..."
  },{
    "name": "data2",
    "values": [ ... ]
  }],
  "sequence" : [ {
    "name": "stream-1",
    "data": {"name": "data1"},
    "tone": { ... },
    "encoding": { ... }
  },
  {
    "name": "stream-2",
    "data": {"name": "data2"},
    "tone": { ... },
    "encoding": { ... }
  }],
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let dataset1 = new Erie.Dataset("data1");
dataset1.set("url", "...path1...");
let dataset2 = new Erie.Dataset("data2");
dataset2.set("values", [...]);

let stream1 = new Erie.Stream();
stream1.data.set("name", "data1");
stream1.tone.set( ... );
stream1.encoding.{channelName1}.set( ... );
stream1.encoding.{channelName2}.set( ... );
...
let stream2 = new Erie.Stream();
stream1.data.set("name", "data2");
stream2.tone.set( ... );
stream2.encoding.{channelName1}.set( ... );
stream2.encoding.{channelName2}.set( ... );
...
let sequence = new Erie.Sequence(stream1, stream2);

{% endhighlight %}
</code-group>
</code-groups>
