---
layout: future
title: Overlay
parent: Stream
level: 1
order: 203
version: future
---

A `overlay` composition plays its child streams together, or simultaneously, or parallelly.
Child streams can share the same dataset and transforms or not (up to you!).

### Default case (common dataset)

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "data": {"url": "...path..."},
  "transform": [
    {...},
    {...}
  ]
  "overlay" : [
    {
      "name": "stream-1",
      "tone": { ... },
      "encoding": { ... }
    },
    {
      "name": "stream-2",
      "tone": { ... },
      "encoding": { ... }
    },
  ]
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream1 = new Erie.Stream();
stream1.tone.set( ... );
stream1.encoding.{channelName1}.set( ... );
stream1.encoding.{channelName2}.set( ... );
...
let stream2 = new Erie.Stream();
stream2.tone.set( ... );
stream2.encoding.{channelName1}.set( ... );
stream2.encoding.{channelName2}.set( ... );
...
let overlay = new Erie.Overlay(stream1, stream2);
overlay.data.set("url", "...path...");
let transform1 = new Erie.{transform}();
let transform2 = new Erie.{transform}();
overlay.transform.add(transform1);
overlay.transform.add(transform2);
{% endhighlight %}
</code-group>
</code-groups>

### Using different datasets (diretly defined)

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "overlay" : [
    {
      "name": "stream-1",
      "data": {"url": "...path1..."},
      "tone": { ... },
      "encoding": { ... }
    },
    {
      "name": "stream-2",
      "data": {"url": "...path2..."},
      "tone": { ... },
      "encoding": { ... }
    },
  ]
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream1 = new Erie.Stream();
stream1.data.set("url", "...path1...");
stream1.tone.set( ... );
stream1.encoding.{channelName1}.set( ... );
stream1.encoding.{channelName2}.set( ... );
...
let stream2 = new Erie.Stream();
stream2.data.set("url", "...path2...");
stream2.tone.set( ... );
stream2.encoding.{channelName1}.set( ... );
stream2.encoding.{channelName2}.set( ... );
...
let overlay = new Erie.Overlay(stream1, stream2);

{% endhighlight %}
</code-group>
</code-groups>

### Using different datasets (with `dataset`)

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
    "url": "...path2..."
  }],
  "overlay" : [ {
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
dataset2.set("url", "...path2...");

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
let overlay = new Erie.Overaly(stream1, stream2);

{% endhighlight %}
</code-group>
</code-groups>
