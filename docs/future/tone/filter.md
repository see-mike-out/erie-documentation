---
layout: future
title: Audio Filter
parent: Tone
level: 1
order: 504
version: future
---

The `filter` property of a `tone` specifies the filters attached to a tone (don't be confused with data transform filters).

If you proivde two filters, then they are connected in chain to the audio context.
For example, if your filters are [`filter1`, `filter2`], then the connection is made as: your tone instrument -> `filter1` -> `filter2` -> `audioContext`.

## Supported preset filters

Standard API is not supported, instead use `Erie.Channel` constructor.

### Sample filter

- `'gainer'`: Simple gain filter (extra channel: `gain2` (`loudness`-type))

### Biquad filters

- `'lowpass'`: Simple lowpass-type biquad filter
- `'highpass'`: Simple highpass-type biquad filter
- `'bandpass'`: Simple bandpass-type biquad filter
- `'lowshelf'`: Simple lowshelf-type biquad filter
- `'highshelf'`: Simple highshelf-type biquad filter
- `'peaking'`: Simple peaking-type biquad filter
- `'notch'`: Simple notch-type biquad filter
- `'allpass'`: Simple allpass-type biquad filter

Biquad filters have the following extra channels

- `biquadDetune`: `detune`-type
- `biquadPitch`: `pitch`-type
- `biquadQ`: [Q factor](https://en.wikipedia.org/wiki/Q_factor) (should range from 0.0001 to 1000, but needs to be specified).
- `biquadGain`: `loudness`-type (only for `'lowshelf'` and `'highshelf'`)

### Dynamic Compressor

- `defaultCompressor`: A default dynamic compressor with (attack = 20, knee = 10, ratio = 18, release = 0.25, threshold = -50)

Additional encoding channels when using a `defaultCompressor` filter.
While some channels have unit of seconds, they are not affected by `config.timeUnit`.

- `dcAttack`: the time taken to have the compression, with the value range of `[0, 1]` (unit: seconds)
- `dcKnee`: the dB range (i.e., from the threshold to knee) for smoothing, with the value range of `[0, 40]`
- `dcRatio`: the amount of change in gain, with the value range of `[1, 20]`
- `dcRelease`: the time taken to resolve the compression, with the value range of `[0, 1]`
- `dcThreshold`: the dB value above which the compression has the effect, with the value range of `[-100, 0]`

See [this documentation](https://developer.mozilla.org/en-US/docs/Web/API/DynamicsCompressorNode) for them.

### Convolver

- `distortion`: A static distortion filter.

### API usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "tone": {
    "type": "default",
    "continued": false,
    "filter": ["lowpass", "gainer"]
  },
  "encoding": {
    "time": { ... },
    "biquadQ": {
      "field": "values",
      "type": "quantitative"
      "scale": {
        "range": [150, 700]
      }
    }
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
let tone = new Erie.Tone("default", false);
tone.addFilter(["lowpass", "gainer"]);
stream.tone.set(tone);
...
stream.encoding.biquadQ = new Erie.Channel("values", "quantitative");
stream.encoding.biquadQ.scale("range", [150, 700]);
...
{% endhighlight %}
</code-group>
</code-groups>

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

3. (Optional) If you want to specify how filter parameters change over time,
then create and register an encoder function with three argutments: `filter` (the filter object),
`sound` (an audio graph queue item), and `startTime` (when the sound starts).
These values are determined by the player.

{% highlight js %}
function MyFilterEncoder(filter, sound, startTime) {
  filter.prop1.linearRampAtTime(sound[x], startTime); // existing channel
  filter.prop1.linearRampAtTime(sound.others[y], startTime); // custom channel
}

Erie.registerFilter('myFilter', MyFilter, MyFilterEncoder);
{% endhighlight %}

4. (Optional) If you want to specify how to change filter parameters that are applied at the end of a sound,
then create and register an finisher function with four argutments: `filter` (the filter object),
`sound` (an audio graph queue item), `startTime` (when the sound starts), `endTime` (when the sound ends).
These values are determined by the player.

{% highlight js %}
function MyFilterFinisher(filter, sound, startTime, endTime) {
  filter.prop1.linearRampAtTime(sound[x], endTime - 0.5); // existing channel
  filter.prop1.linearRampAtTime(sound.others[y], endTime - 0.5); // custom channel
}

Erie.registerFilter('myFilter', MyFilter, MyFilterEncoder, MyFilterFinisher);
{% endhighlight %}