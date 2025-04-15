---
layout: future
title: Stream
level: 0
order: 200
version: future
---

## Terminology: Sequence, Overlay, Stream, Sound, and Tone

In Erie, a `stream` is a unit specification for a sonification, similar to a facet or view in visualizations.
Multiple `stream`s can be combined as a `sequence` or an `overlay` (see the relevant documentations).
Repeated `stream` (with a `repeat` channel in `encoding`) is still a `stream` or repeated `stream`.
The term, `sound`, is an actual output audio.
A `tone` is a unit sound that has no puase (except the pause between two tapping sounds) in it.
For a continuous tone, itself is a sound; whereas for a discrete tone, it is multiple sounds compose a `stream`.


## Composition 

Erie offers two primary options for concatenation-based composition: `sequence` and `overlay`.

### Sequence: concatenate streams serially

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

### Overlay: play streams parallelly

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
- ✅ An `overlay` of `stream`s (*without* a `repeat` channel by `overlay`)

The following patterns are not supported (mostly because it may cause unexpected browser malfunctioning).

- ❌ An `overlay` of `sequences`s. Use `sequences` of `overlay` instead.
- ❌ An `overlay` of `stream`s (**with** a `repeat` channel by `overlay`).
