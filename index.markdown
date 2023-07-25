---
layout: default
title: Home
level: 0
order: 0
---

## Erie

Erie is a 'fully open-sourced' declarative grammar for data sonification!
**It is solely for data sonification, not as an accessibility attachment to a visualization.**
But, you can use it for accessible visualization.

### What Erie supports

- Writing a sonification specification
- Playing a sonification

### What Erie does *NOT* do

- Making a "good" sonification → It's your job to do!

## Try online with examples

Try [Erie online editor]() with examples—the best way to learn how to write an Erie specification.

## Want to help with improving Erie?

(I'm working on it. Currently, please make an issue in the [GitHub repo]().)

There are major future extension plans:

- Connecting to R/Rstudio for statisticians.
- Recording—This is a huge hurdle because it's nearly impossible to programmatically capture an audio (composed of multiple sequences of Web Audio and Web Speech API-generated sounds) at the current moment.

## Some notes

### Open-source projects and standard APIs used in Erie

Erie for Web uses W3C's standard APIs for audio: [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) and [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API).

Many parts of Erie relies on [Vega/Vega-Lite](https://vega.github.io/) and [D3](https://d3js.org/). (Thousands of thousands of Thank you for them).

For URL-based imports, Erie uses [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

### Precision of timing

In most cases, tones are timed/scheduled to be played as indicated.
However, the timing of resulting sonification may not be of high precision in some environments when there are too many tones to play at the same time.
For example, if you use a `discrete` tone for more than 100 data points withint too short time, then the Web Audio API can be overloaded, so it fails to play such sound (it might cause your browsers to be shut down).
In that case, use a `continuous` tone.

### Recoding

The recoding of a resulting sonification is not well streamlined because it is not possible to recorde sound generated from Web Speech API.
In case you have access to different audio generation APIs for non-web enviroments, use the `compileToAudioGraph` method to get the compiled speech and sound values.
