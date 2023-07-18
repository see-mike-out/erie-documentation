---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Encoding
level: 0
order: 700
---

The `encoding` property of a `steam` defines how to map data variables to auditory variables.
While Erie uses many Vega-Lite conventions, each encoding channel may have different attributes and usage patterns.
This is because data sonification is defined on a sound space.
For example, a `time` channel plays two roles: an encoding channel and a sonification canvas at the same time. 

This documentation describes overall structure of each encoding channel and their common properties.

## Common encoding channel properties

| Property | Type | Description |
| --- | ----------- | ----------- |
| `field` | `string` or `string[]` | (Mostly required) A field to encode in a channel. Only the `repeat` channel can use an array expression. |
| `type` | `string` | (Required, but auto-detectable) The data type of a field. Allowed values: `nominal`, `ordinal`, `quantitative`, and `temporal`. |
| `aggregate` | `string` | (Optional) The data aggregation method for the channel. For count-based aggregate methods, `field` is ignored. See `aggregate` documentation for details. |
| `bin` | `boolean` or `object` | (Optional) For an autoamted binning, set as `true`. For more detailed bin settings, see below. |
| `scale` | `object` | (Optional, but highly suggested) The detail of scaling. See below for details. |

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

### Scale