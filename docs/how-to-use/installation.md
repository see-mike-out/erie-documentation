---
layout: default
title: Installation
parent: How to use
level: 1
order: 101
---

## Node

To install for Node projects (e.g., Svelte), run the belwow line in your shell.

- From NPM (Stable)

{% highlight shell %}
npm install erie-web
{% endhighlight %}

- From GitHub (Latest)

{% highlight shell %}
npm install https://github.com/see-mike-out/erie-web.git
{% endhighlight %}


Then, you can import Erie like below.

{% highlight js %}
impor * as Erie from 'erie-web';
{% endhighlight %}

## Browser

Download `erie-web.js` or `erie-web.min.js`.

Have the below imports in your HTML file or wherever it applies to.

{% highlight html %}
<script src="https://cdn.jsdelivr.net/npm/arquero@latest"></script>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js"></script>
<script src="{path}/erie-web.js"></script>
<!-- <script src="{path}/erie-web.min.js"></script>  -->
{% endhighlight %}

Write your script.

{% highlight html %}
<script>
  // Erie is the global object.
</script>
{% endhighlight %}
