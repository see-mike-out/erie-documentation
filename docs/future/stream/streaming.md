---
layout: future
title: Streaming
parent: Stream
level: 1
order: 205
version: future
---

A streaming stream shares most of the properties with a static stream.

To make a sonification for streaming data, **set `stream: true` in your `data` property**.

Some limitations:

- No overlay is supported (yet; future plan).
- Sequnece stream is purposefully not supported. Use a `repeat` channel instead.

## Streaming sonification properties

There are two new properties at **a top-level spec**: `playback` and `notify`.
For `data`, you can provide an optional `test` data.
For `tone`, you can specify a base tone (default: `false`).

### `playback` property

Setting the `playback` property allows for playing some historical data points.
If not provided, no playback will happen (same as setting `init_by: 'manual'`)

| Property | type | Description |
| -------- | ---- | ----------- |
| `init_by` | `'manual'|'conditional'|'always'` | (Optional) How to initiate a playback (`'manual'`: manually–not going to be played unless indicated, `'conditional'`: when a set condition is met, `'always'`: always before playing the new data points). If not provided, no playback will be available.  |
| `condition` | `ExprString` | (Optional) A condition statement for when to play historical data points (see `transform`-`filter` for how to write a condition). |
| `limit` | `number|datetime` | (Optional, default: `3`) The number of historical items to play at a time. If the `unit` is `'time'` then it has to be date-time format. |
| `unit` | `'datum'|'instance'|'time'` | (Optional, default: `'instance'`) The unit of playback limit (`'datum'`: individual data points regardless of when they were received; `'instance'`: data sets received together, `'time'`: up to a certain time that data have been received). They will be queried as instances. |
| `instrument` | `string` | (Optional, default: `sine`) A separate timbre for playback. If the `timbre` channel is used, this is overridden. |
| `speed` | `number` | (Optional, default: `1.5`) Speed factor for playback. The higher, the faster. |

### `notify` property

You can specify five notification settings: `incoming` (to signal new incoming data points), `beforePlayback` (to indicate there is a playback), `afterPlayback` (to indicate the playback is finished), `beforePlay` (to singal new data points being played), `afterPlay` (to mark that the play is done), `next` (to indicate a next sound).
Each playback item can be either `speech`, `sampling`, and `chime` (default).
To not play a notification, set it `null`.

#### `speech` notify (default)

| Property | type | Description |
| -------- | ---- | ----------- |
| `speech` | `string` | (Required) A natural language speech for anouncement. |
| `loudness` | `number[0, 1]` | (Optional, default: `1`) The loudness of the speech. |
| `pitch` | `number` | (Optional, default value follows the browser/TTS function's setting) The pitch of the speech. |
| `language` | `BCP47Expression` | (Optional, default: `'en-US'`) The language for the speech |
| `speechRate` | `number` | (Optional, default: `1`) The speed of the speech. The higher, the faster. |

Default notification values (speech rate = 1, speech in English):

- `incoming`: "Old data" (English)
- `beforePlayback`: "End of old data" (English)
- `afterPlayback`: "Incoming" (English)
- `beforePlay`: "New data" (English)
- `afterPlay`: "End of new data" (English)
- `next`: "Next in history" (English)

These default options are porvided only when set `true` instead of an object.
For any custom speech notification, `speech` property is required.

#### `sampling` notify

| Property | type | Description |
| -------- | ---- | ----------- |
| `sample` | `string` | (Required) A URL for the sampled sound. |
| `loudness` | `number[0, 1]` | (Optional, default: `1`) The loudness of the sound. |
| `detune` | `number[-1200, 1200]` | (Optional, deafult: `0`) The amount of detune for the sound. |

#### `chime` notify (default)

| Property | type | Description |
| -------- | ---- | ----------- |
| `chime` | `ChimeName` | (Required) The name of a chime to use. |
| `loudness` | `number[0, 1]` | (Optional, default: `1`) The loudness of the sound. |

Supported chime names: `'beforePlay'`, `'afterPlay'`, `'beforePlayback'`, `'afterPlayback'`, `'incoming'`, and `'next'`.
They are mapped to the corresponding notification items.
(Custom chime will be supported in the future.)

## Cases

### Case 1: discrete sound without base tone

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "title": "...",
  "description": "...",
  "data": {
    "stream": true,
    "test": {"url": "http://..."}
  },
  "transform": [...],
  "tick": [ ... ],
  "tone": { "continued": false },
  "encoding" : { ... },
  "config": { ... },
  "playback": {
    "init_by": "always",
    "limit": 3,
    "unit": "instance",
    "speed": 2
  },
  "notify": {
    "beforePlay": {
      "speech": "Playing",
      "language": "en-US"
    }
  }
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
stream.title.set("...");
stream.description.set("...");
stream.data.streaming("url": "http://...") // test data;
stream.transform.add(...);
stream.tone = new Erie.Tone();
stream.tone.continued(false); // optional.
...
stream.playback = new Erie.Playback('always', 'instance', 3, 2) // init_by, unit, limit, speed, (condition)
stream.notify.beforePlay = new Erie.Notify("speech"); 
// stream.notify.beforePlay.type("speech") // alternative
stream.notify.beforePlay.speech("Playing").language("en-US");
{% endhighlight %}
</code-group>
</code-groups>

### Case 2: continuous sound with a base tone

For how to set a base tone, go to the `tone` page.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  "title": "...",
  "description": "...",
  "data": {
    "stream": true,
    "test": {"values": [ ... ]}
  },
  "transform": [...],
  "tick": [ ... ],
  "tone": {
    "continued": true,
    "hasBaseTone": true
  },
  "encoding" : {
    "pitch": { "value": 220, ... },
    "loudness": { "value": 0.2, ... } // base values
  },
  "config": { ... },
  "playback": {
    "init_by": "conditional",
    "condition": "datum.value > 5"
  }
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
stream.title.set("...");
stream.description.set("...");
stream.data.streaming([ ... ]) // test data;
stream.transform.add(...);
stream.tone = new Erie.Tone();
stream.tone.continued(true).hasBaseTone(true); // optional.
...
stream.encoding.pitch = new PitchChannel();
stream.encoding.pitch.value(220);
stream.encoding.loudness = new LoudnessChannel();
stream.encoding.loudness.value(0.2);
stream.playback = new Erie.Playback('conditional') // init_by
stream.playback.condition("datum.value > 5");
{% endhighlight %}
</code-group>
</code-groups>
