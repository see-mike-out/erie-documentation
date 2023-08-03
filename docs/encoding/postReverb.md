---
layout: default
title: Post Reverb Channel
parent: Encoding
level: 1
order: 607
---

The `postReverb` is a less loud sound followed by the main tone, and a data variable is mapped to the length of a post-reverb sound.
For example, if the domain is `[10, 20]` and the range is `[0, 2]` (unit: seconds), 
then after the main tone, a 1-second less loud sound is played for a data value of 15.
Note: the ratio of loudness of a post-reverb sound to its main tone will be adjustable soon.

### `postReverb` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "postReverb": {
      "field": "weight",
      "type": "quantitative",
      "scale": {
        "doamin": [10, 20], // optional
        "range": [0, 2] // optional (if omitted, [0, 4])
      }
    }
  }
  ...
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
stream.enc.postReverb.field("weight", "quantitative");
stream.enc.postReverb.scale("domain", [10, 20]); // optinal
stream.enc.postReverb.scale("range", [0, 2]); // optional  (if omitted, [0, 4])
...
{% endhighlight %}
</code-group>
</code-groups>

<!-- todo: example -->