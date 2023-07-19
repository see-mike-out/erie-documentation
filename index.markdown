---
layout: default
title: Home
level: 0
order: 0
---



## What to know

Erie for Web uses W3C's standard APIs for audio: [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) and [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API).

### Precision of timing

In most cases, tones are timed/scheduled to be played as indicated.
However, the timing of resulting sonification may not be of high precision in some environments when there are too many tones to play at the same time.
For example, if you use a `discrete` tone for more than 100 data points, then the Web Audio API can be overloaded, so not paly the sound.
In that case use a `continuous` tone. 


### Recoding

The recoding of a resulting sonification is not well streamlined because it is not possible to recorde sound generated from Web Speech API.
In case you have access to different audio generation APIs for non-web enviroments, use the `compileToAudioGraph` method to get the compiled speech and sound values.