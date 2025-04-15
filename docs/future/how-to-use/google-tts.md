---
layout: future
title: Using Google Cloud TTS
parent: How to use
level: 1
order: 108
version: future
---

Instead of Web Seech API, you can use [Google Cloud Text-to-Speech (TTS) API](https://cloud.google.com/text-to-speech).

# Prerequisite

To do so, you need to complete installation and initialization steps as specified in [this documentation](https://cloud.google.com/text-to-speech/docs/create-audio-text-client-libraries).
Make sure to complete up to "Install the client library" step.

- Create a Google Cloud account
- Create a Text-to-Speech API credentials (you should have a project id at this point).
- Install the Google Cloud CLI
- Initialize it
- Create local authentication credentials for your Google Account (you will use a browser to sign in to your Google cloud console)

Note that Google Cloud TTS only works on a server environment. If you try to include the following APIs, you will run into 403 error or Type error depending on the framework of your choice.

# API

## `GoogleCloudTTSGenerator(sound, config)`

This function returns an `AudioBuffer` of the synthesized speech.

### `sound` (`object`)

The `sound` object has the following properties.

- `speech` (`string`, required): A text to synthesize.
- `language` (`string`, required if necessary): a BCP-47 code (or other standards) for language (default: `en-US`).
- `pitch` (`number`, optional): A detune amount ranging from -20 to 20. 0 is the default detune amount. You could use data generated from a prerendered/compiled queue item.
- `speechRate` (`number`, optinoal): A speed of speech that is greater than 0. (0 is not acceptable). 1 is the default speed. If you don't specify, 1 is used.

### `config` (`object`)

The `config` object has the following properties.

- `language` (optinoal): Same as above. If specified in `sound`, `config.language` is ignored.
- `speechRate` (optinoal): Same as above. If specified in `sound`, `config.language` is ignored.
- `ssmlGender` (`string`, optional): The gender of the output voice. This should be either `NEUTRAL` (default), `FEMALE`, or `MALE`.
- `audioEncoding` (`string`, optional): The output audio file encoding. Refer to the [Google Cloud documentation](https://cloud.google.com/text-to-speech/docs/reference/rest/v1/AudioConfig#audioencoding). The default is `MULAW` (`wav` format). Technically, setting it as `MP3` should not affect the use of `AudioQueue.getFullAudio` method.

### An example using SvelteKit

#### Write a `POST` method function in your `routes/api/TTS/+server.js` file

{% highlight js %}
import { json } from "@sveltejs/kit";
import { GoogleCloudTTSGenerator } from "erie-web";
import { writeFile } from 'node:fs/promises'

export async function POST({ request }) {
  const data = await request.json();
  const sound = data.sound,
    config = data.config,
    options = data.options;
  let response = await GoogleCloudTTSGenerator(sound, config);

  if (options?.getBlob) {
    let blob = new Blob(response, { type: "audio/wav" })
    var url = window.URL.createObjectURL(blob);
    return url;
  } else {
    if (options?.writeFile) {
      let filename = options?.filename || sound.speech.replace(/\s/gi, '-') + '.wav';
      if (options?.path) {
        filename = options.path + filename;
      }
      await writeFile(filename, response, 'binary');
    }
    return json(response);
  }
}
{% endhighlight %}

#### Have a function in your client side file, like `routes/.../+page.svelte`

{% highlight html %}
<script>
  import { bufferToArrayBuffer } from "erie-web";
  async function getTTS(data) {
  let sound = {};
  Object.assign(sound, data.text);
  if (!sound.language) sound.language = document?.documentElement?.lang;
  let res = await fetch("/api/TTS", {
   method: "POST",
   body: JSON.stringify({ sound, config: data.config }),
   headers: {
    "content-type": "application/json",
   },
  });
  let parsed = await res.json();
  return bufferToArrayBuffer(parsed.data);
 }

  let BlobLink;
  async function getFullAudio() {
    BlobLink = await queue.getFullAudio(getTTS);
  }
</script>

<a href={totalBlobLink}>Download</a>
{% endhighlight %}

This code will save output audio in your server directory.
