---
title: Imperial Units in CSS
abstract: Sometimes the old ways are best.
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---
## RIP SASS, LESS, & Stylus
For the last umpteen years I have been using Stylus, a concise DSL to avoid writing CSS directly. Popular alternatives, especially outside the JavaScript world, are Sass and LESS.

These tools actually don't solve a problem with CSS as a language. Their main benefit is generating vendor-specific prefixes for browser compatibility. Case in point, [autoprefixer](https://www.npmjs.com/package/autoprefixer-core), a tool that does only that, has over a million downloads per month.

Other features, like nesting or variables, come at the cost of incompatibility, stack bloat, and make it harder for others to approach the code base.

Meanwhile the CSS standard has been modularised. Many new features are being proposed. Waiting until they are supported in all browsers is sub-optimal. Wouldn't it be nice if we could do what Traceur and Babel did for ECMAScript?

## Hello, PostCSS.
PostCSS is a *post*-processor that transpiles *CSS to CSS*.

That's right.

PostCSS is simply a (very efficient) CSS parser with hooks for plug-ins to mutate the CSS output. And therein lies its power.

There are already dozens of PostCSS plug-ins and packs for anything from linting to minification to supporting English spelling (e.g. `colour`). One of my favourites is cssnext which transpiles experimental features to currently supported CSS.

Naturally I had to create my own plug-in for PostCSS. This turned out to be very easy by using the [Plugin Boilerplate](https://github.com/postcss/postcss-plugin-boilerplate). I chose to create a silly plug-in for fun, as my objective was to learn the PostCSS API more than create something useful.

## postcss-imperial
What my [postcss-imperial](https://www.npmjs.com/package/postcss-imperial) plug-in offers is the use of British Imperial and US Customary length units of measurement.

Haven't you always wanted to define font sizes in multiples of a poppyseed? No? Well, now you can!

{% highlight css %}
div {
  font-size: 2poppyseeds;
  border: 1barleycorn solid wheat;
}
{% endhighlight %}

Perhaps this will be useful when doing print design. Who knows.

This is a very basic plug-in. It iterates through all CSS values matching an imperial unit using the [replaceValues API](https://github.com/postcss/postcss/blob/master/docs/api.md#containerreplacevaluespattern-opts-callback).

The lengths are replaced with equivalent inches or points, which are still imperial units and officially part of the [CSS Values and Units Module](http://www.w3.org/TR/css-values/). Win.

{% highlight js %}
css.replaceValues(pattern, { fast: unit }, translate)
{% endhighlight %}

Easy, right?

More advanced plug-ins can add or remove entire rules, using source maps to keep track of changes.

Now if only there was a plug-in to support decimetres and decametres. Hmmm...
