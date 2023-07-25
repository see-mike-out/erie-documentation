---
layout: default
title: Stream
level: 0
order: 200
---

## Terminology: Sequence, Overlay, Stream, Sound, and Tone

In Erie, a `stream` is a specification for a unit of sonification, similar to a facet in visualizations.
Multiple `stream`s can be combined as a `sequence` or an `overlay` (see the Sequence documentations).
Repeated `stream` (with a `repeat` channel in `encoding`) is still a `stream` or repeated `stream`.
The term, `sound`, is an actual output audio.
A `tone` is a unit sound that has no puase (except the pause between two tapping sounds) in it.
For a continuous tone, it is the entire sound; whereas for a discrete tone, it is each sound composes a `stream`.

A `stream` can have the following porperties.

## Stream properties

| Property | type | Description |
| -------- | ---- | ----------- |
| `title` | `string` | (Required) The title of a sonification (if specified, it is played) |
| `description` | `string` | (Required) The description of a sonification (if specified, it is played). |
| `data` | `object` | (Required) Data to sonify. |
| `transform` | `array` | Data to sonify. |
| `tone` | `object` | Tone definition. |
| `encoding` | `object` | (Required) Encoding channels. |
| `config` | `object` | Configuration object. |

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
stream.enc.{channelName} = channel;
// ex) let channel = new Erie.PitchChannel(); // provided for reusability.
//     stream.enc.pitch.set(channel);
// alt) let channel = new Erie.Channel({channelName}); // recommended
// alt) stream.enc.{channelName}.method( ... );
// take a look at each individual channel's documentation.
stream.config({key}, {value});
{% endhighlight %}
</code-group>
</code-groups>
