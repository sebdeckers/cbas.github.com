---
title: Template Literals
abstract: A few simple tricks using this ECMAScript 6.0 feature.
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---
## Template Literals vs String Literals
Literals are ways to write data types regardless of their actual value in memory. For example the boolean `true` or `false` values are written as full words, yet represent only a single bit each.

String literals are simply Unicode code points wrapped in single or double quotes, with optional backslash escaped characters mixed in.

Recently ECMAScript 6.0 introduced a new way to define strings: Template literals. These are enclosed in back ticks. Templates may contain substitutions which are snippets of code in between `${` and `}`.

## Substitutions
Any code in the substitution is evaluated and converted to a string. Let's look at some simple benefits this offers.

### Nested Variables
Inspect object properties.
{% highlight js %}
`Listening on port ${server.recv}`
// Listening on port 1337
{% endhighlight %}

### Invoking Methods
{% highlight js %}
`It is currently ${ new Date().toLocaleString() }`
// It is now 09/07/2015, 10:09:36
{% endhighlight %}

### Multi-Line Code Structuring
Inline code into the substitution while keeping it easy to read, not stretching endlessly on one line.
{% highlight js %}
`Hello ${ // This is a comment and won't appear in the output
  message.to
    .trim()
    .replace(/\d/g, '')
}, ${message.body}`
// Hello Bob, ...
{% endhighlight %}

### Concatenating Arrays
Use `Array.prototype.toString` to comma-join arrays.
{% highlight js %}
const scopes = ['friends', 'messages', 'likes']
`https://api.example.com/login?scopes=${scopes}`
// https://api.example.com/login?scopes=friends,messages,likes
{% endhighlight %}

### Custom toString
Implement your own `toString` method.
{% highlight js %}
const person = {
  title: 'Mr.',
  name: 'Sebastiaan',
  toString: function () {
    return this.title + ' ' + this.name
  }
}
`Welcome, ${person}`
// Welcome, Mr. Sebastiaan
{% endhighlight %}
