---
layout: future
title: Static Streams
parent: Stream
level: 1
order: 201
version: future
---

A `stream` can have the following porperties.

## Stream properties

### Case 1: a single sonification (top-level)

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `DataSpec` | Data to sonify. |
| `transform` | `Array<TransformSpec>` | Data transformations to apply. |
| `tone` | `ToneSpec` | (Required) Tone definition. |
| `encoding` | `{[channel]: ChannelSpec}` | (Required) Encoding channels. |
| `tick` | `Array<TickSpec>` | Registering ticks to be used (mainly for multi-stream sonifications). |
| `sampling` | `Array<SamplingSpec>` | Registering sampling audios to be used. |
| `synth` | `Array<SynthSpec>` | Registering synths sounds to be used. |
| `wave` | `Array<WaveSpec>` | Registering periodic waves to be used. |
| `config` | `Object` | Configuration object. |
| `ordering` | `Array<OrderSpec>` | Configuration object. |

Note: `datasets`, `sampling`, `synth`, and `tick` defined not in the top-level stream will be ignored.

### Case 2: a Sequence sonification

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `DataSpec` | Data to sonify (if provided, it functions as the dataset for all the unit streams, and `datasets` should not be provided). |
| `datasets` | `Array<DataSpec>` | Registering datasets to be used (if provided `data` should not be provided). |
| `transform` | `Array<TransformSpec>` | Data transformations to apply. |
| `stream` | `Array[UnitStream|Overlay]` | Streams in the sequence. |
| `sampling` | `Array<SamplingSpec>` | Registering sampling audios to be used. |
| `synth` | `Array<SynthSpec>` | Registering synths sounds to be used. |
| `wave` | `Array<WaveSpec>` | Registering periodic waves to be used. |
| `config` | `Object` | Configuration object. |
| `ordering` | `Array<OrderSpec>` | Configuration object. |

### Case 3: an Overlay sonification

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `Object` | Data to sonify (if provided, it functions as the dataset for all the unit streams, and `datasets` should not be provided). |
| `datasets` | `Array` | Registering datasets to be used (if provided `data` should not be provided). |
| `transform` | `Array<TransformSpec>` | Data transformations to apply. |
| `stream` | `Array[UnitStream]` | Streams in the overlay. |
| `sampling` | `Array<SamplingSpec>` | Registering sampling audios to be used. |
| `synth` | `Array<SynthSpec>` | Registering synths sounds to be used. |
| `wave` | `Array<WaveSpec>` | Registering periodic waves to be used. |
| `config` | `Object` | Configuration object. |
| `ordering` | `Array<OrderSpec>` | Configuration object. |

Note: `datasets`, `sampling`, `synth`, and `tick` defined not in the top-level stream will be ignored.

### Unit stream properties

A `UnitStream` does not have registrations.

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `name` | `String` | (Purely optional) An identifier (it is not used for sonification itself). |
| `data` | `Object` | Data to sonify. |
| `transform` | `Array` | Data to sonify. |
| `tone` | `Object` | (Required) Tone definition. |
| `encoding` | `Object` | (Required) Encoding channels. |
| `config` | `Object` | Configuration object. |

## API Usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "title": "...",
  "description": "...",
  "data": { ... },
  "transform": [...],
  "tick": [ ... ],
  "encoding" : { ... },
  "config": { ... }
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
stream.title.set("...");
stream.description.set("...");
stream.data.set(...);
stream.transform.add(...);
stream.transform.remove(index); // see the transform documentation
stream.tone = new Erie.Tone();
// alt) stream.tone({key}, {value});
let channel = new Erie.{ChannelName}Channel();
stream.encoding.{channelName} = channel;
// ex) let channel = new Erie.PitchChannel(); // provided for reusability.
//     stream.encoding.pitch.set(channel);
// alt) let channel = new Erie.Channel({channelName}); // recommended
// alt) stream.encoding.{channelName}.method( ... );
// take a look at each individual channel's documentation.
stream.config({key}, {value});
{% endhighlight %}
</code-group>
</code-groups>
