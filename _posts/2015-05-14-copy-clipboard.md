---
layout: post
title:  "Copy to clipboard"
date:   2015-05-14 10:15:20
categories: javascript bootstrap
---

I recently worked on a web app that present code snippets to the user, so I though it would be nice to have a "copy to clipboard" feature. Saving a few clicks is always welcome!

So first thing to note is the lack of a standard *javascript* function for that.
The most common way is to use an invisible **flash** movie, which means it won't work on a mobile devise (and you need to handle it gracefully).

I will use [ZeroClipboard](https://github.com/zeroclipboard/zeroclipboard), which is the most popular library that implement this flash hack.
To make it looks nice I will use [Bootstrap](http://getbootstrap.com/)

### Screenshot
<img alt="Screenshot" src="/assets/images/copy-to-clipboard.png" style="width: 650px; margin: 0 0 0 0;">

#### HTML
{% highlight html %}
<div class="code-box">
    <span class="btn btn-default btn-sm btn-clipboard pull-right" data-clipboard-target="code">
        Copy
    </span>
    <pre id="code">
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));
    </pre>
</div>
{% endhighlight %}

#### Javascript
{% highlight javascript %}
// zeroclipboard object
var clip = new ZeroClipboard($('.btn-clipboard'));
var clipElems = $(clip.elements());
// tooltip logic
var tooltipOptions = {title: 'Copy to clipboard', placement: 'top'};
clipElems.tooltip(tooltipOptions);
clipElems.click(function() {
    clipElems.data('bs.tooltip').options.title = 'Copied!';
    clipElems.tooltip('show');
});
clipElems.mouseleave(function () {
    clipElems.data('bs.tooltip').options.title = tooltipOptions.title;
});
{% endhighlight %}

#### CSS
{% highlight css %}
.btn-clipboard {
    position: relative;
    right: 51px;
    border-radius: 0 4px 0 4px;

}
.zeroclipboard-is-hover {
    color: #fff;
    border-color: #563d7c;
    background-color: #563d7c;
}
{% endhighlight %}

I created a [Gist](https://gist.github.com/damsonn/dde446c5abe4f0dcbe06) with the full example.