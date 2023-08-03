---
layout: default
title: Audio Filter
parent: Tone
level: 1
order: 504
---

The `filter` property of a `tone` specifies the filters attached to a tone (don't be confused with data transform filters).

If you proivde two filters, then they are connected in chain to the audio context.
For example, if your filters are [`filter1`, `filter2`], then the connection is made as: your tone instrument -> `filter1` -> `filter2` -> `audioContext`.

## Supported preset filters (no additional encoding is supported)

- `'gainer'`: Simple gain filter

## How to create a custom filter

Note: This is not easy!

See this [documentation](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Advanced_techniques) for technical understanding.

1. Make a filter class with a constructor that takes an audio context.
A filter class must have the following methods: `connect` and `disconnect`.

a) If you want your filter to ecnode data points, then the relevant properties must have that property and it should be defined as (or similarly to) an [AudioParam](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam).
For instance, if you have a custom channel of `custom1`,
then this should have `name`, `defaultValue`, `minValue`, `maxValue`, and `value` properties and
`setValueAtTime`, `setTargetAtTime`, `setValueCurveAtTime`, `linearRampAtTime`, `exponentialRampToValueAtTime`, `cancelAndHoldAtTime`, `cancelScheduledValues` methods.
At least, it should have `name`, `value`, `defaultValue` properties and `setValueAtTime`, `setTargetAtTime`,`linearRampAtTime`, `exponentialRampToValueAtTime` methods.

EX) `filter.custom1.value = ...`, `filter.custom1.setValueAtTime(..., ...)`, `filter.custom1.setTargetAtTime(..., ...)`.

b) The `connect` and `disconnect` methods should be properly connect/disconnect audio nodes.

{% highlight js %}
class MyFilter {
  constructor(ctx) {
    this.context = ctx; // audio context
    this.amount = {
      name: 'amount'
      value: ...,
      defaultValue: ...,
      setValueAtTime: function(... ... ...) { ... },
      setTargetAtTime: function(... ... ...) { ... },
      linearRampAtTime:  function(... ... ...) { ... },
      exponentialRampToValueAtTime: function(... ... ...) { ... },
      ...
    }; // you can also make it as a separate class
  }
  initialize(time) {
    // if provided, it runs right after a filter is instantiated.
    // the `time` argument is when the filter starts. (It is determined by the player).
    ...
  }
  connect(node) {
    ...
  }
  disconnect(node) {
    ...
  }
}
{% endhighlight %}

2. Register this class to Erie.

{% highlight js %}
Erie.registerFilter('myFilter', MyFilter);
{% endhighlight %}

3. (Optional) If you want to specify how a filter parameters change over time,
then create and register an encoder function with four argutments: `filter` (the filter object),
`sound` (an audio graph queue item), and `startTime` (when the effect starts).
These values are determined by the player.

{% highlight js %}
function MyFilterEncoder(filter, sound, startTime) {
  filter.prop1.linearRampAtTime(sound[x], startTime); // existing channel
  filter.prop1.linearRampAtTime(sound.others[y], startTime); // custom channel
}

Erie.registerFilter('myFilter', MyFilter, MyFilterEncoder);
{% endhighlight %}
