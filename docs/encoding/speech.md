---
layout: default
title: Speech (before and after) Channels
parent: Encoding
level: 1
order: 608
version: current
---

A speech channel enocdes a natural language sound before (`speechBefore`) or after (`speechAfter`) the main tone.
These channels works for discrete (only) and relatively timed (ideally) tones.
The speech rate is controled by the `config` object.

### `speechBefore` usage pattern

<code-groups>
<code-group>
<h4>JSON</h4>
{% highlight json %}
{
  ...
  "encoding" : {
    "speechBefore": {
      "field": "name",
      "type": "nominal",
      "language": "en-US"
    }
  }
  ...
  "config": {
    "speechRate": 1.75 // default
  }
}
{% endhighlight %}
</code-group>
<code-group>
<h4>JavaScript</h4>
{% highlight js %}
let stream = new Erie.Stream();
...
stream.encoding.speechBefore.field("name", "nominal");
stream.encoding.speechBefore.language("en-US");
...
stream.config("speechRate", 1.75);
{% endhighlight %}
</code-group>
</code-groups>

### Language

Using the `language` property (in [BCP-47 format](https://www.techonthenet.com/js/language_tags.php#:~:text=BCP%2047%20Language%20Tags%20is,region%2C%20variant%20and%20script%20subtags.)), one can set the language of a speech.
The default language is determined by the browser document's language.
For example if a web page states `<html lang="en">`, then the default language can be `en-GB` (British English).
Because this channel uses Web Speech API (see [this MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance/lang)), supported languages include 
Arabic (Soudi Arabia), Bangla (Bangladesh, India), Czech, Danish, German (Germany, Austria, Switzerland), Greek, English (Australia, Canada, UK, Ireland, India, New Zealand, United States, South Africa), Spanish (Spain, Argentina, Chile, Columbia, Mexico, United States), Finnish, French (France, Belgium, Canada, Switzerland), Hebrew,
Hindi, Hungarian, Indonesian, Italian (Italy, Switzerland), Japanese, Korean, Dutch (The Netherlands, Belgium), Norwegian, Plish, Portugese (Portugal, Brazil), Romanian, Russian, Slovak, Swedish, Tamil (India, Sri Lanka), Thai, Turkish, Chinese-Mandarin (China, Taiwan) and Chinese-Cantonese (Hong Kong).
