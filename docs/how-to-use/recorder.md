---
layout: default
title: Recorder (Chrome Extension)
parent: How to use
level: 1
order: 106
---

You can record the sonfication and save it using Erie's chrome extension (link-anonymized).

## For general users

This section is temporarily removed due to anonymization.

### What you get?

After finishing playing a sonification, the extension will automatically start download output files.
They include an HTML document, an SSML document, and associated audio files in the `webm` format.

## For developers

To use, first download the extension via `[hidden]`.

Second, install the extension via developer tool ([how to](https://developer.chrome.com/docs/extensions/mv3/getstarted/development-basics/#load-unpacked)).

## Technical limits

Unfortunately, it is impossible to record sound generated from your own browser (even though it is your active tab) for security, privacy, and copyright purposes.
So, Erie recorder uses Chrome's extension API, called `tabCapture` (sadly, there is no Firefox equivalence—Please let us know if there's a workaround).
It also uses `tabs` and `downloads` APIs for identification and auto-download functionalities.

### Full technical issues with recording/capturing

I hope this gives you a full picture about this workaround recorder.
Before that, we make a distinction between recording and capturing.
Recording means obtaining external sound through an audio input device (such as a microphone attached to your phone or computer).
Capturing means obtaining the sound that from the source of an audio output device (such as a speaker attached to your phone or computer).
It is not impossible to record the sonification output; I am aware that there are some solutions that enable "record" your sound on the online (I have searched solutions over months).
However, there are serious drawbacks by doing so.
First, it also records any noise in the environment.
Second, if you have to use an earphone, that the recording fails as your microphone cannot hear the sound from your earphone.
Third, it can technically do okay job with the speech sounds generated using the SpeechSynthesis, but by default recording the sound from your speaker(i.e., the output to the input of the same device), which I assume is the most common case, causes a lot (a lot really) of audio feedback, resulting in distorted audio quality, particularly with sounds generated using the Web Audio API.
Thus, I chose to use this capture method instead as it provides better audio quality and speech can be read using a screen reader or SpeechSynthsis.
