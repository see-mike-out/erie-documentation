---
layout: default
title: How to Use
level: 0
order: 100
---

Erie is a data sonification grammar, and Erie has Spec API, queue compiler, and player for Web, written in JavaScript.
To use Erie, you first need to write a sonification design spec in JSON or using Spec API.
Then, you pass the spec to the queue compiler, and play the sound.

## Example usage

The below example describes a simple use case of Erie for Web.

{% highlight js %}
let spec = ...
compileAuidoGraph(spec) // (1) compile to a queue
  .then((audio_graph) => {
    let audio = audio_graph;
    audio.prerender() // (2) prerender before playing (optional)
      .then((q) => {
        audio.playQueue(); // (3) play entire sonification
      })
      .catch((e) => {
        console.warn(e);
      });
  })
  .catch((e) => {
    console.warn(e);
  });
{% endhighlight %}
