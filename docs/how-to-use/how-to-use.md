---
layout: default
title: How to Use
level: 0
order: 100
version: current
---

Erie.js has a spec API, queue compiler, and player for web, written in JavaScript.
A first step of using Erie.js is to write a sonification design spec in JSON or using the spec API.
Then, you pass the spec to the queue compiler, and play the resulting queue.

## Example usage

The below example describes a simple use case of Erie for Web.

{% highlight js %}
let spec = ...; (0) write a spec
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
