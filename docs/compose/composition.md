---
layout: default
title: Composition
level: 0
order: 700
---

Erie offers two primary options to compose a multi-stream sonification: `sequence` and `overlay`.

## Sequence: concatenate streams serially

Multiple streams concatenated using `sequence` are played one by one.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "sequence" : [
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
let stream1 = new Erie.Stream();
...
let stream2 = new Erie.Stream();
...

let sequence = new Erie.Sequence(stream1, stream2);
// alternatively
let sequence = new Erie.Sequence([stream1, stream2]);
{% endhighlight %}
</code-group>
</code-groups>

## Overlay: play streams parallelly

Multiple streams joined using `overlay` are played simultaneously together.
Note: using a speech encoding channel may result in uncoordinated scheduling.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "overlay" : [
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
let stream1 = new Erie.Stream();
...
let stream2 = new Erie.Stream();
...

let overlay = new Erie.Overlay(stream1, stream2);
// alternatively
let overlay = new Erie.Overlay([stream1, stream2]);
{% endhighlight %}
</code-group>
</code-groups>

## Patterns

Erie supports the following patterns for composing multiple streams.

- ✅ A `sequence` of `overlay`s and/or `stream`s
- ✅ A `overlay` of `stream`s (without a `repeat` channel by `overlay`)

The following patterns are not supported (mostly because it may cause unexpected browser malfunctioning).

- ❌ A `overlay` of `sequences`s. Use `sequences` of `overlay` instead.
- ❌ A `overlay` of `stream`s (with a `repeat` channel by `overlay`).
