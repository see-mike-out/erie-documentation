---
layout: default
title: Stream
level: 0
order: 200
---

## Terminology: Sequence, Overlay, Stream, Sound, and Tone

In Erie, a `stream` is a unit specification for a sonification, similar to a facet or view in visualizations.
Multiple `stream`s can be combined as a `sequence` or an `overlay` (see the relevant documentations).
Repeated `stream` (with a `repeat` channel in `encoding`) is still a `stream` or repeated `stream`.
The term, `sound`, is an actual output audio.
A `tone` is a unit sound that has no puase (except the pause between two tapping sounds) in it.
For a continuous tone, itself is a sound; whereas for a discrete tone, it is multiple sounds compose a `stream`.

A `stream` can have the following porperties.

## Stream properties

### Case 1: a single sonification (top-level)

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `Object` | Data to sonify. |
| `transform` | `Array` | Data to sonify. |
| `tone` | `Object` | (Required) Tone definition. |
| `encoding` | `Object` | (Required) Encoding channels. |
| `tick` | `Array` | Registering ticks to be used (mainly for multi-stream sonifications). |
| `sampling` | `Array` | Registering sampling audios to be used. |
| `synth` | `Array` | Registering synths sounds to be used. |
| `wave` | `Array` | Registering periodic waves to be used. |
| `config` | `Object` | Configuration object. |

Note: `datasets`, `sampling`, `synth`, and `tick` defined not in the top-level stream will be ignored.

### Case 2: a Sequence sonification

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `Object` | Data to sonify (if provided, it functions as the dataset for all the unit streams, and `datasets` should not be provided). |
| `datasets` | `Array` | Registering datasets to be used (if provided `data` should not be provided). |
| `stream` | `Array[UnitStream|Overlay]` | Streams in the sequence. |
| `sampling` | `Array` | Registering sampling audios to be used. |
| `synth` | `Array` | Registering synths sounds to be used. |
| `wave` | `Array` | Registering periodic waves to be used. |
| `tick` | `Array` | Registering ticks to be used (mainly for multi-stream sonifications). |
| `transform` | `Array` | Data to sonify. |
| `config` | `Object` | Configuration object. |

### Case 3: an Overlay sonification

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `String` | The title of a sonification (if specified, it is played) |
| `description` | `String` | The description of a sonification (if specified, it is played). |
| `data` | `Object` | Data to sonify (if provided, it functions as the dataset for all the unit streams, and `datasets` should not be provided). |
| `datasets` | `Array` | Registering datasets to be used (if provided `data` should not be provided). |
| `stream` | `Array[UnitStream]` | Streams in the overlay. |
| `sampling` | `Array` | Registering sampling audios to be used. |
| `synth` | `Array` | Registering synths sounds to be used. |
| `wave` | `Array` | Registering periodic waves to be used. |
| `tick` | `Array` | Registering ticks to be used (mainly for multi-stream sonifications). |
| `transform` | `Array` | Data to sonify. |
| `config` | `Object` | Configuration object. |

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
