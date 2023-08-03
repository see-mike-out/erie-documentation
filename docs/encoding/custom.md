---
layout: default
title: Custom channels
parent: Encoding
level: 1
order: 610
---

One can provide custom channels that can be attached to user specified filters.

It only applies to the audio filters that you add to your tone.

Make sure that always specify the range of your scale, otherwise it might fail to render.

### Custom channel API usage

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "gain2": {
      "field": "Body Mass (g)",
      "type": "quantitative",
      "scale": {
        "doamin": [0, 7000], // optional
        "range": [0, 1] // required
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
class Gain2Channel extends Erie.Channel {
  constructor(f, t) {
    super(f, t);
    this._channel = 'gain2';
  }

  validator(v) {
    return ... ; // validates a range value
  }
}

let stream = new Erie.Stream();
...
stream.enc.gain2 = new Gain2Channel();
stream.enc.gain2.domain([0, 7000]);
stream.enc.gain2.range([0, 1]);
...
{% endhighlight %}
</code-group>
</code-groups>
