---
layout: default
title: Encoding
level: 0
order: 700
---

The `encoding` property of a `steam` defines how to map data variables to auditory variables.
While Erie uses many Vega-Lite conventions, each encoding channel may have different properties and usage patterns.
This is because data sonification is defined on a sound space.
For example, a `time` channel plays two roles: an encoding channel and a sonification canvas at the same time.

This documentation describes overall structure of each encoding channel and their common properties.

## Supported encoding channels

| Channel | Description |
| ------- | ----------- |
| `time` | (Required) The time when a tone is played. |
| `time2` | The time when a tone is finished, with a data field sharing the same scale with the corresponding `time` channel. |
| `duration` | The length of a tone being played, with a data field *not* sharing the same scale with the corresponding `time` channel. |
| `tapSpeed` | The number of tapping sounds uniformly distributed over the specified duration of time. |
| `tapCount` | The number of tapping sounds of the same specified length. |
| `pitch` | The pitch frequency of a tone. |
| `loudness` | The volume or loudness of a tone. |
| `pan` | The stereo panning (left and right spatial positioing) of a tone. |
| `postReverb` | The length of the reverb sound played after a tone. |
| `speechBefore` | The speech text played before playing a tone. |
| `speechAfter` | The speech text played after playing a tone. |
| `repeat` | Repeating streams of the same structure (i.e., encoding and tone design) over one or more `nominal`/`ordinal` variables. |

### Encoding channels planned for updates

| Channel | Description |
| ------- | ----------- |
| `modulation` | The degree of modulation of a FM synth tone. |
| `panX` | For 3D panning, the left-right position of a tone. While it is same as `pan` but it works in a 3D panning setting. Requires a high-end audio device and advanced computing device, otherwise using 3D panning will cause high latency or low quality. |
| `panY` | For 3D panning, the front-back position of a tone. |
| `panZ` | For 3D panning, the top-down position of a tone. |

## Common encoding channel properties

| Property | Type | Description |
| --- | ----------- | ----------- |
| `field` | `string` or `string[]` | (Mostly required) A field to encode in a channel. Only the `repeat` channel can use an array expression. |
| `type` | `string` | (Required, but auto-detected) The data type of a field. Allowed values: `nominal`, `ordinal`, `quantitative`, and `temporal`. |
| `aggregate` | `string` | (Optional) The data aggregation method for the channel. For count-based aggregate methods, `field` is ignored. See `aggregate` documentation for details. |
| `bin` | `boolean` or `object` | (Optional) For an autoamted binning, set as `true`. For more detailed bin settings, see below. |
| `condition` | `object` | Conditional value assignment. |
| `value` | `any` | (Optional) A static sound for a channel. |
| `scale` | `object` | (Optional, but highly suggested) The detail of scaling. See below for details. |
| `speech` | `boolean` | (Optional, for `repeat` channel only, default: `true`) Whether to announce the name of value for the `repeat` channel. |
| `tick` | `object` | (Optional, default: `null`; only for `time` channel) An audio axis. |

### Aggregate

Possible `aggregate` values 

Note: `corr`, `covariance`, and `covariancep` is not available for the encoding `aggregate`.

See the `transform` documentation for further details.

### Bin

The simplest way to use in-channel binning is setting the `bin` property as `true`.
To provide more details, one can use a `bin` object with details.

| Property | Type | Description |
| --- | ----------- | ----------- |
| `nice` | `boolean` | (Optional) Whether to use 'nicely' separated bins (default: `true`). |
| `maxbins` | `integer` | (Optional) The maximum number of bins (default: `10`). |
| `step` | `number` | (Optional) The exact size acrass all bins. |
| `exact` | `number[]` | (Optional) The eaxct bin sizes for each bin. For example, if it is `[0, 10, 15, 20]`, then the resulting bins are `[0, 10)`, `[10, 15)`, and `[15, 20)`. |

### Scale

The `scale` propoerty describes further details about the mapping between the specified data variable and the targeted auditory channel.
A `scale` object can have the following properties.
Each encoding channel may have different `scale` properties, so refer to relevant documentations for details.

| Property | Type | Description |
| --- | ----------- | ----------- |
| `domain` | `any[]` | (Optional) An array of domain intervals. `domain` must have the same length with `range`. The data type should be in accordance with the `field`'s type. |
| `range` | `any[]` | (Optional) An array of range intervals. `range` must have the same length with `doamin`. The data type should be in accordance with the targeted channel's type (e.g., `number` for `pitch`, `string` for `timbre`). |
| `polarity` | `'positive'|'negative'` | (Optional) The mapping direction. If it is `'positive'`, then a higher data value maps to a higher auditory value (e.g., more loud, higher pitch, right pan). If it is `'negative'`, then a higher data value maps to a lower auditory value (e.g., less loud, lower pitch, left pan). This does not work when `domain` and `range` are both provided. |
| `maxDistinct` | `boolean` | (Optional, default: `true`) When `range` is not specified and `maxDistinct` is true, then the `range` becomes the entire range of that channel (e.g., from the lowest pitch to the highest pitch, from the pan of `-1`—leftmost to pan of `1`–rightmost). |
| `times` | `number` | (Optional) A direct scale factor. When provided, for a value `v`, the auditory value is `v * times`. If the `max(value) * times` or `min(value) * times` goes beyond the possible auditory value for each channel, then the specified `times` is adjusted. |
| `zero` | `boolean` | (Optional, default: `false`) Whether to include the zero value in the scale for a quantitative encoding. The default value is `zero` unlike visualization to maximalize the discriminability of auditory values because some audio values have limited possible range. |
| `description` | `'nonskip'|'skip'`,`null`, or`string` | (Optional, default: `'nonskip'`) Whether to play a description about the scale before playing the sonification stream. If it is `'skip'` or `null`, then description is not played. If provided as a string that is not `'skip'` or `'nonskip'`, then the provided string is played instead. |
| `length` | `number` (unit: second) | (Optional, only for the `time` channel) The entire length of a sonfication stream with an absolute timing (a shortcut to `range`). |
| `band` | `number` (unit: second) | (Optional) For the `time` channel, the length of each tone. For the `tapSpeed` channel, the length of time duration. For the `tapCount` channel, the length of each tapping sound. |
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
      "speech": true // or false
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
// or let channel = new Erie.({channelName});
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

stream.enc.{channelName}.set(channel); // provided for reusability;

// method 2 (more direct)
stream.enc.{channelName}.field("Origin", "nominal");
stream.enc.{channelName}.scale("timing", "relative");

// method 1 + 2
stream.enc.{channelName} = new Erie.Channel("Origin", "nominal");
stream.enc.{channelName}.scale("timing", "relative");

{% endhighlight %}
</code-group>
</code-groups>

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
as long as they rely on the same dataset and are of the same encoding `type` (e.g., `nominal`, `quantitative`).
If the overlaid streams use a different dataset or of a different encoding type, 
then the scale will be different. 


##### Case 2: Nested
'Stream-A' and 'Overlay-1' definee a pitch channel, while the others do not. 
Then, 'Overlay-2', and 'Overlay-3' use the same pitch scale as defined in 'Overlay-1' 
as long as they rely on the same dataset and are of the same encoding `type` (e.g., `nominal`, `quantitative`).
If 'Overlay-2', and 'Overlay-3' use a different dataset or of a different encoding type, 
then the scale will be different. 

##### Case 3: I just need individually defined scales
If it is necessary to use individually defined scales, 
then set `overlayScaleConsistency` (for `overlay`) or `sequenceScaleConsistency` (for `sequence`) to be `true`,
in the closest `config` object.
To make inconsistent scales between 'Stream-A' and 'Stream-B', 
then set `sequenceScaleConsistency` as `true` in the top-level `sequence`.
To make inconsistent scales among 'Overlay-1', 'Overlay-2', and 'Overlay-3',
then set `overlayScaleConsistency` as `true` in the `overlay` object.