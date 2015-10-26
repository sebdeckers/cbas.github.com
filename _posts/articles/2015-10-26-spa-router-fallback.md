---
title: Single Page App Routing with Express & Node.js
abstract: Server-side fallback for client-side routers.
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---
Using HTML5 History API (pushSate/popState) makes intra-app navigation fast. URLs change on the client-side and no request–response round-trips are needed, reducing latency to nil.

However the user may refresh the page, restart their browser, or share the link with someone else. In that case a request will be made to the web server for the changed URL. Since single page apps typically only have one entry point file (e.g. `/index.html`), that same file should be served for every valid URL.

## Best Solution
If you Googled your way to this page and demand instant gratification, here you go.

### express-history-api-fallback [![Latest NPM version number of express-history-api-fallback](https://img.shields.io/npm/v/express-history-api-fallback.svg)](https://www.npmjs.com/package/express-history-api-fallback)
{% highlight bash %}
npm install --save express-history-api-fallback
{% endhighlight %}
{% highlight js %}
import fallback from 'express-history-api-fallback'
import express from 'express'
const app = express()
const root = `${__dirname}/public`
app.use(express.static(root))
app.use(fallback('index.html', { root }))
{% endhighlight %}
Or in ECMAScript 5:
{% highlight js %}
var fallback = require('express-history-api-fallback')
var express = require('express')
var app = express()
var root = __dirname + '/pubic'
app.use(express.static(root))
app.use(fallback('index.html', { root: root }))
{% endhighlight %}

### Benefits
- **Only for GET requests**: The fallback should not serve your `index.html` for `POST` or other requests.
- **Only for HTML requests**: Never serve mistakenly for JS or CSS or image or other static file requests. Less debugging headaches.
- **Only when needed**: Serve the fallback only when the file is missing.
- **High performance**: Let `res.sendFile()` in Express `>=4.8.0` do the heavy lifting of serving the file.
- **Minimal code**: Fits on one line. No magic. No complexity.

## Good Solution
Don't want any more dependencies? Want to keep it simple, stupid? Here you go.

This is a fallback so place this code *after* the `express.static(...)` middleware.

{% highlight js %}
app.get((req, res, next) => {
  if (req.accepts('html')) res.sendFile(`${__dirname}/index.html`)
  else next()
})
{% endhighlight %}
Equivalent in ECMAScript 5:
{% highlight js %}
app.get(function (req, res, next) {
  if (req.accepts('html')) res.sendFile(__dirname + '/index.html')
  else next()
})
{% endhighlight %}

## Bad Solutions
Existing sub-optimal middlewares to serve a static entry point file.

### connect-history-api-fallback [![Latest NPM version number of connect-history-api-fallback](https://img.shields.io/npm/v/connect-history-api-fallback.svg)](https://www.npmjs.com/package/connect-history-api-fallback)
This is currently the most popular Connect- and Express-compatible middleware that attempts to solve this problem. And it's badly broken.

- **Broken by design**: **It is not even a fallback!** Placing this middleware *after* the static middleware will have no effect other than rewriting `req.url` for subsequent middlewares.
- **Breaks valid requests**: Placing this middleware *before* the middleware risks breaking valid requests. Request URLs are rewritten regardless of whether or not the file actually exists.
- **Poor defaults**: Any path name containing a dot is ignored. Good luck if your app uses URL slugs (who doesn't?). [The author refused to fix this.](https://github.com/bripkens/connect-history-api-fallback/issues/5)
- **Bloated**: Uses 100 lines of code to implement this tiny feature.
- **Feature creep**: Rewriting URLs should not be part of this module. Do one thing and do it well.

### express-spa [![Latest NPM version number of express-spa](https://img.shields.io/npm/v/express-spa.svg)](https://www.npmjs.com/package/express-spa)
Someone re-implemented this middleware. Hardly any users and not a useful solution.

- **Over engineered**: Over 50 lines including its own file streaming.
- **Poor defaults**: Any path ending with a file extension is ignored. This breaks apps with terminating URL slugs.
- **Bad code**: Uses `!=` instead of `!==`, hard-coded `node_modules` path lookup, creates a function inside a tight loop, checks too many MIME types, does not set file stream encoding, etc.
- **Feature creep**: For no obvious reason performs a key/value substitution on the file contents. KISS.

### express-spa-router [![Latest NPM version number of express-spa-router](https://img.shields.io/npm/v/express-spa-router.svg)](https://www.npmjs.com/package/express-spa-router)
Another unpopular package that fails to solve the problem.

- **Convoluted**: Uses error handlers to serve successful responses. Non-obvious names like `extraRoutes` and `noRoute` where neither are route objects.
- **No testing**: Over 60 lines of naked, untrustable code.
- **Regex madness**: Relies on a combination of whitelisting and blacklisting URL prefix patterns.
