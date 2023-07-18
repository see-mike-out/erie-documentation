---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

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

## Common encoding channel properties

| Property | Type | Description |
| --- | ----------- | ----------- |
| `field` | `string` or `string[]` | (Mostly required) A field to encode in a channel. Only the `repeat` channel can use an array expression. |
| `type` | `string` | (Required, but auto-detected) The data type of a field. Allowed values: `nominal`, `ordinal`, `quantitative`, and `temporal`. |
| `aggregate` | `string` | (Optional) The data aggregation method for the channel. For count-based aggregate methods, `field` is ignored. See `aggregate` documentation for details. |
| `bin` | `boolean` or `object` | (Optional) For an autoamted binning, set as `true`. For more detailed bin settings, see below. |
| `scale` | `object` | (Optional, but highly suggested) The detail of scaling. See below for details. |
| `speech` | `boolean` | (Optional, for `repeat` channel only, default: `true`) Whether to announce the name of value for the `repeat` channel. |

### Aggregate

Possible `aggregate` values are:

- Single-value summaries
  - `mean` or `average`
  - `mode`
  - `median`
  - `sum`
  - `product`
  - `max`
  - `min`

- Dispersion-related
  - `stdev`
  - `stdevp`
  - `variance`
  - `variancep`

- Count-based:
  - `count`
  - `valid`
  - `invalid`
  - `distinct`

Note: `corr`, `covariance`, `covariancep`, and `quartile` is not available for the encoding `aggregate`.

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
| `description` | `'nonskip'|'skip'` | (Optional, default: `'nonskip'`) Whether to play a description about the scale before playing the sonification stream. Future plan is to provide more description options. |
| `length` | `number` (unit: second) | (Optional, only for the `time` channel) The entire length of a sonfication stream with an absolute timing (a shortcut to `range`). |
| `band` | `number` (unit: second) | (Optional, only for the `time` channel) The length of each tone. |
| `timing` | `'relative'|'absolute'` | (Optional) The timing (when it starts) of each tone (default: `'absolute'`). |

## API usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "channel": {
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
        "band": ..., // Seconds
        "timing": "absolute" // or "relative"
      },
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
let channel = new Erie.Channel();
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

stream.enc.ChannelName = channel;

// method 2 (more direct)
stream.enc.ChannelName.field("Origin", "nominal");
stream.enc.ChannelName.scale("timing", "relative");

// method 1 + 2
stream.enc.ChannelName = new Erie.Channel("Origin", "nominal");
stream.enc.ChannelName.scale("timing", "relative");

{% endhighlight %}
</code-group>
</code-groups>