---
layout: default
title: Loudness Channel
parent: Encoding
level: 1
order: 705
---

Note: Erie Web Player does not map the `loudness` to the `volume` of a sound (i.e., actual decibel value) 
because doing so may override of conflict with the user's volume setting.
For instance, a user with low hearing may have to set their device volumen higher than others,
and a user with sensitive hearing may have set it lower than others.
Instead, Erie Web Player maps the `loudness` to the `gain` (or velocity) of a sound (i.e., how strong a sound is played).
This can work with users' different volume settings (i.e., relative volume control).