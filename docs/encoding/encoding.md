---
layout: default
title: Encoding
level: 0
order: 600
---

The `encoding` property of a `steam` defines how to map data variables to auditory variables.
While Erie uses many Vega-Lite conventions, each encoding channel may have different properties and usage patterns.
This is because data sonification is defined on a sound space.
For example, a `time` channel plays two roles: an encoding channel and a sonification canvas at the same time.

This documentation describes overall structure and common properties of encoding channels.

## Supported encoding channels

| Channel | Description |
| ------- | ----------- |
| `time` | (Required) The time when a tone is played. |
| `time2` | The time when a tone is finished, with a data field sharing the same scale with that of the `time` channel. |
| `duration` | The length of a tone being played, with a data field *not* sharing the same scale with that of the `time` channel. |
| `tapSpeed` | The number of tapping sounds evenly distributed over the specified duration of time. |
| `tapCount` | The number of tapping sounds of the same specified length. |
| `pitch` | The pitch frequency of a tone. For a synth instrument, this changes the carrier's frequency. |
| `detune` | (Only for noise tones) The amount of detuning a tone. |
| `loudness` | The volume or loudness of a tone. For a synth instrument, this changes the carrier's frequency. It is controled by gain. |
| `pan` | The stereo panning (left and right spatial positioing) of a tone. |
| `postReverb` | The length of the reverb sound played after a tone. |
| `speechBefore` | The speech text played before playing a tone. |
| `speechAfter` | The speech text played after playing a tone. |
| `repeat` | Repeating streams of the same structure (i.e., encoding and tone design) over one or more `nominal`/`ordinal` variables. |
| `modulation` | Modultaiotn index for a synth tone (the higher the index, the more warping the sound for FM; the larger the sound for AM). |
| `harmonicity` | Harmony between AM carrier and modulator (non-linear channel). |

For more details about the `modulation`, please refer to [frequency modulation](https://en.wikipedia.org/wiki/Frequency_modulation#Modulation_index) and [amplitude modulation (or harmonicity)](https://en.wikipedia.org/wiki/Amplitude_modulation#Modulation_index).

## Common encoding channel properties

| Property | Type | Description |
| --- | ----------- | ----------- |
| `field` | `String` or `Array[String]` | (Mostly required) A field to encode in a channel. Only the `repeat` channel can use an array expression. |
| `type` | `String` | (Required, but auto-detected) The data type of a field. Allowed values: `nominal`, `ordinal`, `quantitative`, and `temporal`. |
| `aggregate` | `String` | (Optional) The data aggregation method for the channel. For count-based aggregate methods, `field` is ignored. See `aggregate` documentation for details. |
| `bin` | `Boolean` or `Object` | (Optional) For an autoamted binning, set as `true`. For more detailed bin settings, see below. |
| `condition` | `Object` | Conditional value assignment. |
| `value` | `any` | (Optional) A static sound for a channel. |
| `scale` | `Object` | (Optional, but highly suggested) The detail of scaling. See below for details. |
| `ramp` | `true|false|'abrupt'|'linear'|'exponential'` | (Optional, for `continuous` tones) Whether/how to ramp audio properties. `true` is `'linear'` and `false` is `'abrupt'`. |
| `speech` | `Boolean` | (Optional, for `repeat` channel only, default: `true`) Whether to announce the name of value for the `repeat` channel. |
| `tick` | `object|string` | (Optional, default: `null`; only for `time` channel) An audio axis. If it is `String`, then it should be a registered tick's name. |
| `format` | `String` | (Optional) The format of numerical values for a scale description or read out. See this D3 documentations ([number format](https://github.com/d3/d3-format), [time format](https://github.com/d3/d3-time-format)) for the expression. |
| `formatType` | `'number'|'datetime'` | (Optional, default: `'datetime'`) The type of formatting. |

### Inline `aggregate`

#### Possible `aggregate` values

- `count`: the count of elements
- `valid`: the number of valid items in a variable
- `distinct`: the number of distinct items in a variable
- `mean`/`average`: the mean of a variable
- `mode`: the mode of a variable
- `median`: the median of a variable
- `quantile`: the `p` quantile value of a variable
- `stdev`: the standard deviation of a variable
- `stdevp`: the population standard deviation of a variable
- `variance`: the variance of a variable
- `variancep`: the population variance of a variable
- `sum`: the sum of a variable
- `product`: the product (multiplication) of a variable
- `max`: the maximum value of a variable
- `min`: the min value of a variable

Note: double-field aggregates (`corr`, `covariance`, and `covariancep`) are not available for inline `aggregate`.

See the `transform` documentation for further details.

### Bin

The simplest way to use in-channel binning is setting the `bin` property as `true`.
To provide more details, one can use a `bin` `Object` with details.

| Property | Type | Description |
| --- | ----------- | ----------- |
| `nice` | `Boolean` | (Optional) Whether to use 'nicely' separated bins (default: `true`). |
| `maxbins` | `integer` | (Optional) The maximum number of bins (default: `10`). |
| `step` | `Number` | (Optional) The exact size acrass all bins. |
| `exact` | `Array[Number]` | (Optional) The eaxct bin sizes for each bin. For example, if it is `[0, 10, 15, 20]`, then the resulting bins are `[0, 10)`, `[10, 15)`, and `[15, 20)`. |

### Scale

The `scale` propoerty describes further details about the mapping between the specified data variable and the targeted auditory channel.
A `scale` object can have the following properties.
Each encoding channel may have different `scale` properties, so refer to relevant documentations for details.

| Property | Type | Description |
| --- | ----------- | ----------- |
| `domain` | `Array[Any]` | (Optional) An array of domain intervals. `domain` must have the same length with `range`. The data type should be in accordance with the `field`'s type. |
| `range` | `Array[Any]` | (Optional) An array of range intervals. `range` must have the same length with `doamin`. The data type should be in accordance with the targeted channel's type (e.g., `Number` for `pitch`, `String` for `timbre`). |
| `polarity` | `'positive'|'negative'` | (Optional) The mapping direction. If it is `'positive'`, then a higher data value maps to a higher auditory value (e.g., more loud, higher pitch, right pan). If it is `'negative'`, then a higher data value maps to a lower auditory value (e.g., less loud, lower pitch, left pan). |
| `maxDistinct` | `Boolean` | (Optional, default: `true`) When `range` is not specified and `maxDistinct` is true, then the `range` becomes the maximum-possible range of that channel (e.g., from the lowest pitch to the highest pitch, from the pan of `-1`—leftmost to pan of `1`–rightmost). |
| `times` | `Number` | (Optional) A direct scale factor. When provided, for a value `v`, the auditory value is `v * times`. If the `max(value) * times` or `min(value) * times` goes beyond the possible auditory value for each channel, then the specified `times` is adjusted. |
| `zero` | `Boolean` | (Optional, default: `false`) Whether to include the zero value in the scale for a quantitative encoding. The default value is `false`, so as to maximalize the discriminability of auditory values because some audio values have limited possible range. |
| `description` | `'nonskip'|'skip'`,`null`, or`descriptionMarkup` | (Optional, default: `'nonskip'`) Specifying the description of a scale. If it is `'skip'` or `null`, then description is not played. If provided as a string that is not `'skip'` or `'nonskip'`, then the provided string or markked-up expression is played instead. See below for the description markup. |
| `title` | `String` | (Optional, default is the field's name) If provided, it replaces the field name in the default description. |
| `length` | `Number` (unit: second) | (Optional, only for the `time` channel) The entire length of a sonfication stream with an absolute timing (a shortcut to `range`). |
| `band` | `Number` (unit: second) | (Optional) For the `time` channel, the length of each tone. For the `tapSpeed` channel, the length of time duration. For the `tapCount` channel, the length of each tapping sound. |
| `timing` | `'relative'|'absolute'|'simultaneous'` | (Optional) The timing (when it starts) of each tone (default: `'absolute'`). |

#### API usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "[channel]": {
      "field": "FieldName",
      "type": "DataType",
      "aggregate": "AggregateMethod", // only for a `quantitative` channel
      "bin": {
        "nice": true // or false,
        "maxbins": 10,
        "step": 5, // Number
        "exact": false // or true
      }, // or true or false
      "scale": {
        "domain": [ ... ],
        "range": [ ... ],
        "polarity": "positive" // or "negative",
        "maxDistinct": true // or false,
        "times": ..., // Number,
        "zero": false // or true,
        "description": "nonskip" // or "skip",
        // only for `time` channel
        "length": ..., // Seconds,
        "timing": "absolute", // or "relative"
        // only for `time`, `tapSpeed`, and `tapCount` channels
        "band": ..., // Seconds,
      },
      "value": ...,
      "condition": [
        {"test": [ value1, value2, ... ], "value": ... }, // as array
        {"test": {"not": [ value1, value2, ... ]}, "value": ... }, // "not"
        {"test": "d.field > 0", "value": ... } // conditional
      ],
      "speech": true // or false,
      "format": '.2', // (rounding at the second decimal point), no default value
      "formatType": "number" // default value
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

// method 1 (more reusable)
let channel = new Erie.{ChannelName}();
channel.field("Origin", "nominal");
// or let channel = new Erie.Channel("Origin", "nominal");
let maxbins = 10, nice = true, step;
channel.bin(maxbins, step, nice);
// channel.bin(true);
// channel.bin(false);
// let exact = [0, 10, 15, 20];
// channel.bin(exact);
channel.aggregate("mean");
channel.scale("timing", "relative"); // key & value
channel.scale("domain", [0, 10]);
channel.scale("timing", "relative");

// static
channel.value(...);
channel.condition.add([ value1, value2, ... ], ...);
channel.condition.add({"not": [ value1, value2, ... ]}, ...);
channel.condition.add("d.field > 0", ...);

// format
channel.format('.2', 'number');
channel.format('.2');
channel.formatType('number');

stream.encoding.{channelName}.set(channel); // provided for reusability;

// method 2 (more direct)
stream.encoding.{channelName}.field("Origin", "nominal");
stream.encoding.{channelName}.scale("timing", "relative");

// method 1 + 2
stream.encoding.{channelName} = new Erie.Channel("Origin", "nominal");
stream.encoding.{channelName}.scale("timing", "relative");

{% endhighlight %}
</code-group>
</code-groups>

#### Scale description markup

For the `description` property of a scale, you can provide a marked-up expression
to be read out before playing the sonification or be separately played.

##### How to mark up a description text

There are three four types of elements in a description text.

1. Plain text → Just give that text
2. A discrete audio legend → `<sound value="X" duration="D">`

- `X`: a "data" value or reserved keywoard to be mapped using the scale.
- `D`: the duration of this sound (default: 0.5 seconds or 1 beat). If `config.timeUnit` is `beat` then the unit is beat counts, and when it is `second`, then the unit is seconds.

3. A continuous audio legend → `<sound v0="X1" v1="X2" duration="D">`

- `vn`: `Xn` (`n` = `0`, `1`, `2`, ...): breakpoints, the "data" values or reserved keywoards to be mapped using the scale! Provide as many point as you need. They will be timed relatively.
- `D`: the duration of this sound (default: 0.5 seconds or 1 beat multipled by the number of breakpoints). If `config.timeUnit` is `beat` then the unit is beat counts, and when it is `second`, then the unit is seconds.

4. A datum list → `<list item="A" first="F" last="L" join="S" and="N">`

- `A`: an array/list of items or a keyword to be listed. If not provided, all the domain values will be sounded. They should be comma separated.
For example, `<list item="apple,pear, juice" ... >` (O) but `<list item="[apple,pear, 'juice']" ... >` (may work, but may cause error). Spaces before and after each item is trimmed.
- `F`: the first `F` items to read out
- `L`: the last `F` items to read out

Note: `<list item="apple,pear,juice,melon,grape" first="2" last="2" join=", ">` will result in 'apple, pear, melon, grape'.
See the examples for how to use.

- `S`: a string between each consecutive pair of items. This considers a space (for iternationalization). The default is `,`
- `N`: a string between the last two consecutive items. This considers a space (for iternationalization). There is no default value provided.

Note: For the `time` channel, while the rest can be "compiled," it won't really play the audio.

##### Reserved keywords

To use in a plan text, use them with `<` and `>` (e.g., `<domain.min>`)

- `domain.min`: the minimum domain value
- `domain.max`: the maximum domain value
- `domain.length`: the number of domain values specified
- `domain[n]`: the `n`-th domain value
- `domain`: the entire domain
- `channel`: the channel name
- `field`: the channel's field
- `aggregate`: the channel's aggregate method
- `title`: the channel's display name (if not provided, it uses `field`).

###### Examples

Case 1: Consider a `pitch` channel that maps `[0, 50]` (data, field: `'weight'`) to `[300, 500]` (Hz).
The description text is

`The <channel> channel represents <title>. The minimum value <domain.min> is <sound value="domain.min"> and <domain.max> is <sound value="domain.max">.`
This is compiled to *The pitch channel represents weight. The minimum value 0 is (sound with pitch 300) and 50 is (sound with pitch 500).*

Case 2: Consider a `pan` channel that maps `[100, 300]` (data, field: `'height'`) to `[-1, 1]`.
The description text is

`The <channel> channel is for <title>. The minimum value of <domain.min> and the maximum value of <domain.max> are mapped to <sound v0="domain.min" v1="domain.max" duration="0.3">.`

This is compiled to *The pan channel is for height. The minimum value 100 and the maximum value of 300 are mapped to (pan-sweep from -1 to 1 for 0.6 seconds).*

Case 3: Consider a `time` channel that maps `[apple,pear,kiwi,melon,grape,mango,orange]` (data, field: `'type'`) to relative timing.
The description text is

`The <field> has <domain.unique> itmes. They are played from <list first="2" join=" and "> to <list last="2" join=" and ">.`

This is compiled to *The type has 7 items. They are played from apple and pear to mango and orange.*

##### How to play a description

1. Play it before a sonification

Quick answer: Nothing to be done.

2. Play it separately

In `config`, first set `skipScaleSpeech` as `true`.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "config": {
    "skipScaleSpeech": true
  }
}
{% endhighlight %}
</code-group>

<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let spec = new Erie.Stream();
...
spec.config('skipScaleSpeec', true);
{% endhighlight %}
</code-group>
</code-groups>

After you compile the sonification spec, run the `playDescription` method.

{% highlight js %}
let spec = new Erie.Stream();
...
let audio;
Erie.compileAuidoGraph(spec)
  .then((audio_graph) => {
    let audio = audio_graph;
    ...
    audio.playScaleDescription();
    audio.playScaleDescription('time'); // play only for the time channel
    audio.stopScaleDescription(); // to stop whatever being played
  });
...
{% endhighlight %}

If `scale.description` is set to `'skip'` or `null`, then it is also not played with the `playScaleDescription` method.

#### Scale consistency for multi-stream sonifications

It is possible (and default) to use a consistent scales unless otherwise specified.

For example, suppose a `sequence` consisting of one single `stream` and one three-stream `overlay`, so there are four streams defined.
Let's name them 'Stream-A', 'Overlay-1', 'Overlay-2', and 'Overlay-3', respectively.
'Overlay-1', 'Overlay-2', and 'Overlay-3' comprise 'Stream-B'
The specification will look like:
`{"sequence": [ {... Stream-A ...}, { "overlay": [Overlay-1, Overaly-2, Overlay-3]}]}`.

Then, assume these streams use a pitch channel.

##### Case 1: Global

'Stream-A' defines a pitch channel, while the others do not.
Then, 'Overlay-1', 'Overlay-2', and 'Overlay-3' use the same pitch scale as defined in 'Stream-A'
as long as they rely on the same dataset and field, and are of the same encoding `type` (e.g., `nominal`, `quantitative`).
If the overlaid streams use a different dataset or of a different encoding type,
then the scale will be different.

##### Case 2: Nested

'Stream-A' and 'Overlay-1' define a pitch channel, while the others do not.
Then, 'Overlay-2', and 'Overlay-3' use the same pitch scale as defined in 'Overlay-1'
as long as they rely on the same dataset and field, and are of the same encoding `type` (e.g., `nominal`, `quantitative`).
If 'Overlay-2', and 'Overlay-3' use a different dataset, field, or encoding type,
then the scale will be different.

##### Case 3: I just need individually defined scales

If it is necessary to use individually defined scales,
then set `overlayScaleConsistency` (for `overlay`) or `sequenceScaleConsistency` (for `sequence`) to `false`,
in the closest `config` object.
To make inconsistent scales between 'Stream-A' and 'Stream-B',
then set `sequenceScaleConsistency` as `false` in the top-level `sequence`.
To make inconsistent scales among 'Overlay-1', 'Overlay-2', and 'Overlay-3',
then set `overlayScaleConsistency` as `false` in the `overlay` object.

When two scale definitions conflicts, a property defined earliest is applied.

This can also done by channels, for example `{ "sequenceScaleConsistency": { "pitch" : false } }`.
