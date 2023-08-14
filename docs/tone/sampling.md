---
layout: default
title: Sampling
parent: Tone
level: 1
order: 502
---

It is possible to sample sounds by providing sampling sounds.
Note that Erie uses the [`fetch` API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) for getting the sound.
These instrument types do *NOT* suppport `continued` tones.

## Sampling multi-pitch sounds

If you want to support multi-pitch instrument, you may need to sample multiple sounds of C note in each scale (from 0 to 7).
Ideally, prepare 2-3 seconds of a sampled sound.

**Why?** Tuning a sampled sound distors the sampled wave form (e.g., changing the quality/length of the form).
The larger the amount of tuning, the bigger the distortion is.

Note: for a sampled sound, the pitch is capped from C0 to F7#.

If your `pitch` channel's range is lower than the C7 note scale, then provide only needed sample audios.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "sampling": [
    {
      "name": "sample_audio",
      "sample": {
        "C0": ..., // url string
        "C1": ..., // url string
        "C2": ..., // url string
        "C3": ..., // url string
        "C4": ..., // url string
        "C5": ..., // url string
        "C6": ..., // url string
        "C7": ... // url string
      }
    }
  ],
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let tone = new Erie.SampledTone("sample_audio", {
  "C0": ..., // url string
  "C1": ..., // url string
  "C2": ..., // url string
  "C3": ..., // url string
  "C4": ..., // url string
  "C5": ..., // url string
  "C6": ..., // url string
  "C7": ... // url string
});
stream.sampling.add(tone); // optional
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>

## Sampling single-pitch sounds

For single pitch sounds like drum, use the `mono` property.

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "sampling": [
    {
      "name": "sample_audio",
      "sample": {
        "mono": ...  // url string
      }
    },
  ],
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
let tone = new Erie.SampledTone("sample_audio", {mono: ...}); // url string
stream.tone.set(tone);
...
{% endhighlight %}
</code-group>
</code-groups>
