---
layout: default
title: Custom channels
parent: Encoding
level: 1
order: 610
---

A developer can provide custom channels that can be attached to their specified audio filters.

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
// this is a bit extended usage case for extra safety.
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
stream.encoding.gain2 = new Gain2Channel();
// alternatively, (it does not check the range values)
// stream.encoding.gain2 = new Erie.Channel("gain2");
stream.encoding.gain2.domain([0, 7000]);
stream.encoding.gain2.range([0, 1]);
...
{% endhighlight %}
</code-group>
</code-groups>
