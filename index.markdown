---
layout: default
title: Home
level: 0
order: 0
---

## Erie (Web)

Erie for Web (or simply Erie.js) is a web implementation of Erie, a declarative grammar for data sonification.
**Erie.js is intended for supporting sonification, not as auxiliary for visualization.**

### What Erie.js supports

- Writing a sonification specification
- Playing a sonification

### What Erie.js does *NOT* do

- Making a "well-designed/good" sonification → It's your job to do!

## Some notes

### Open-source projects and standard APIs used in Erie

Erie for Web uses W3C's standard APIs for audio: [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) and [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API). For URL-based imports, Erie uses the standard [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

Many parts of Erie relies on [Vega/Vega-Lite](https://vega.github.io/) and [D3](https://d3js.org/). (Thousands of thousands of Thank you for them).

### Precision of timing

In most cases, tones are timed/scheduled to be played as indicated.
However, the timing of resulting sonification may not be of high precision in some environments when there are too many tones to play at the same time.
For example, if you use a `discrete` tone for more than 100 data points withint too short time, then the Web Audio API can be overloaded, so it fails to play such sound (it might cause your browsers to be shut down).
In that case, use a `continuous` tone.

### Recording

The Web APIs do not support directly capturing audio generated from your browser or desktop (presumably for security, privacy, and copyright reasons).
Instead, we use a Chrome extension by utilizing Chrome's `tabCapture` API to generate audio files out of Erie sonifications.
Yet, it is still not possible to capture the sounds generated using the Web Speech API (`SpeechSynthesis`) because technically it is not generated from your browser but from your computer. Instead, our recorder creates an HTML file that has the text parts and recorded sounds so that they can be played using a screen reader.
