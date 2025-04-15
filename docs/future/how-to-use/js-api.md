---
layout: future
title: JavaScript Spec API
parent: How to use
level: 1
order: 102
version: future
---

#### 1. To use Erie.js spec API, first create an Erie Spec object

{% highlight js %}
let spec = new Erie.Stream();
{% endhighlight %}

#### 2. Then, specify your design using our API

This documentation contains how to write both JavaScript and JSON-based specs.

#### 3. The third step is to `get` the spec, and pass it to the compiler

{% highlight js %}
compileAuidoGraph(spec.get());
{% endhighlight %}

## Common Methods

Each of Erie's Spec API classes (e.g., `Stream`, `Sequence`, `Tone`) have the following common methods.

### `SpecInstance.get()`

The `get` method returns a JSON object describing the instance.

### `SpecInstance.clone()`

The `clone` method returns the cloned, deep-copied instance.

For class-specific methods, refer to relevant documentations.
In most cases, you can use each property name of a class as a method to set the corresponding value.
