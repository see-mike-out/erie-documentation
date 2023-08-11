---
layout: default
title: Recorder (Chrome Extension)
parent: How to use
level: 1
order: 106
---

You can record the sonfication and save it using [Erie's chrome extension](https://github.com/see-mike-out/erie-chrome-ext).

## For general users

### Install

Search "Erie Recorder for Chrome" on Chrome Web Store and install it.

### Use

Open the extension popup. Then play a sonification.

### What you get?

After finishing playing a sonification, the extension will automatically start download output files.
They include an HTML document, an SSML document, and associated audio files in the webm format.

## For developers

To use, first download the extension via `https://github.com/see-mike-out/erie-chrome-ext`.

Second, install the extension via developer tool ([how to](https://developer.chrome.com/docs/extensions/mv3/getstarted/development-basics/#load-unpacked)).

## Why is it like this?

Honestly, I have the same question.

Unfortunately, it is impossible to record sound generated from your own browser (even though it is your active tab) for security, privacy, and copyright purposes.
So, Erie recorder uses Chrome's extension API, called `tabCapture` (sadly, there is no Firefox equivalence).
It also uses `tabs` and `downloads` APIs for identification and auto-download functionalities.
