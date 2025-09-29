---
layout: future
title: Ordering
level: 0
order: 900
version: future
---

To customize the play order of a sonification and add custom speech elements between sound elements, use the `ordering` property at the top level.
When the `ordering` is provided, only specified elements will be played.
When not provided, a defualt ordering spec is generated.
Once compiled, the ordering spec is "normalized." To check and debug the normalized ordering, use the `SequenceStream.ordering` property.

## Writing the `ordering` property of a stream

Essentially, the `ordering` property is an `Array` of `OrderItem`s.
Each `OrderItem` is either `Markup`, `Sound`, and `Text` item.

### `OrderItem` 1-`Markup`: Non-soninfication elements referring to the stream spec(s)

This is useful for specifying an order item for scale descriptions.

| Property | type | Description |
| -------- | ---- | ----------- |
| `specifier` | `Specifier` | (Required) A reference property/element in the stream spec(s). |
| `markup` | `MarkupText | Array<MarkupText> | { [key: String]: MarkupText }` | (Optional) Markup elements. See ['scale description markup'](../encoding/encoding.html#scale-description-markup). For object types, `key` is for channel names for specifying the scale descriptions or multiple channels together. If the `specifier` is fully saturated, then can be skipped. |
| `description` | `String` | (Optional) Some notes for developers. |  
| `speechOption` | `SpeechOption` (`Object`) | (Optional) Some configuration objects for speech. |

### `OrderItem` 2: `Sound`: Sonification elements

| Property | type | Description |
| -------- | ---- | ----------- |
| `specifier` | `Specifier` | (Required) A reference property/element in the stream spec(s). |
| `notify` | `NotifyObject` | (Optional) Notification of the sound. See the [`notify` property](../stream/streaming.html#notify-property) in the `streaming` spec page. |
| `description` | `String` | (Optional) Some notes for developers. |  
| `speechOption` | `SpeechOption` (`Object`) | (Optional) Some configuration objects for speech. |

### `OrderItem` 3: `Text`: Custom text elements

| Property | type | Description |
| -------- | ---- | ----------- |
| `text` | `String` | (Required) A text string to speak out. |
| `description` | `String` | (Optional) Some notes for developers. |  
| `speechOption` | `SpeechOption` (`Object`) | (Optional) Some configuration objects for speech. |

### Specifier

A `specifier` is to indicate an element (e.g., sonification sound, scale description) from the sonification design spec.
A specifier has the follwoing properties.

| Property | type | Description |
| -------- | ---- | ----------- |
| `role` | `String` | (Required) A "role" item (see below). |
| `stream` | `StreamIdentifier` | (Optional) Identifying a stream element. If the sonification has a single (overlay or unit) stream, then it is not required. |  
| `is_repeated` | `Boolean` | (Optional) Whether this specifier is for a repeated stream. |
| `channel` | `String` | (Optional) Indicating a specific channel for a scale description markup item. |

#### `StreamItdentifier`

While all properties are technically optional, either one of those must be provided.

| Property | type | Description |
| -------- | ---- | ----------- |
| `index` | `Integer` | (Required) The index of a sub-stream in the entire sequence. |
| `name` | `String` | (Optional) The name of a sub-stream in the entire sequence. It works only when the sub-stream has a `name`. |  
| `overlay` | `{index?: number, name?: string}` | (Optional) To indicate a sub-stream of an overlay. Either `index` or `name` must be provided. This is for the scale description of a sub-stream, not for the sonification (i.e., sound item). See below for more. |

*The more specified, the more narrowed down.* For example, if no `StreamIdentifier` is provided, then the `role` will indicate the entire sonification.
If provided, then it will locate the related elements of the specified sub-stream.

#### Supported roles

The `role` property of a `specifier` supports the following:

- `stop-play-keyboard-shortcut`: a speech announcement for the keyboard shortcut for stopping the sound.
- `length`: the number of sound streams. For the length of a stream, use `scale.description` instead.
- `title`: the title of a sonification or a sub-stream, depending on the `stream` property of the `specifier`.
- `description`: the description of a sonification or a sub-stream, depending on the `stream` property of the `specifier`.
- `name`: the name of a sonification or a sub-stream, depending on the `stream` property of the `specifier`.
- `scale.overview`: the overview statement of the scales of a sonification or a sub-stream.
- `scale.description`: the description of a chosen scale(s) of a sonification or a sub-stream.
- `sound`: the actual sound of a sonification or a sub-stream.
- `repeat.title`: the title of data-driven repetition sound (i.e., the current values)

### `SpeechOption` object

| Property | type | Description |
| -------- | ---- | ----------- |
| `loudness` | `number[0-1]` | (Optional, default: `1`) The loudness of the speech. |
| `pitch` | `number` | (Optional, default value follows the browser/TTS function's setting) The pitch of the speech. |
| `language` | `BCP47Expression` | (Optional, default: `'en-US'`) The language for the speech |
| `speechRate` | `number` | (Optional, default: `1.75`) The speed of the speech. The higher, the faster. |

## The level of specificity

The `ordering` property is not for specifying a sonification design.
The following actions are purposefully not allowed.

- ❌ Selecting a subset of an overlay stream. → Fix the overlay spec.
- ❌ Defining a new encoding channel. → Fix the stream spec.
- ❌ Defining a short, simple sonification. → Write a sonification stream spec.
