---
title: Why Hypermedia APIs are bullshit
abstract: HATEOAS is not a real solution to real problems
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---

This post was triggered by a talk at [Singapore.JS](http://www.meetup.com/Singapore-JS/) on [Hypermedia APIs](http://en.wikipedia.org/wiki/HATEOAS). Anyone interested should join our fragile little Javascript developer community. ♥

---

I hate HATEOAS. There, I said it. Don't get me wrong, I think RESTful APIs are [the way of the future](http://www.youtube.com/watch?v=4_Pbx9mvWPY). But the Hypermedia API neurosis is a step in the wrong direction.

1. They combine the bloated verbosity of XML with the global namespace collisions of JSON. FML.
1. The concept of hypermedia will lead API developers to abuse HTML. Ignorant assholes will disregard the HTML spec, polluting its namespace, and break the web by overloading the semantic meaning of HTML elements.
1. A fixed, constrained schema, like HTML, introduces ambiguity between rich API response data types. API providers are choked by a limited number of HTML elements.
1. Encourages Copy & Paste crap coding because developers. Developers use URLs that "worked on my machine."
1. Frontend developers don't have a problem of *too much API documentation*. In fact there rarely is documentation *at all*, because *documenting is hard*. The solution is not automated writing of documentation for an implementation (ie. Javadoc verbal diarrhea).
1. It adds more complexity to an API implementation, making it more likely to break the spec, increasing the cost of maintenance, and causing inevitable struggle between future improvements and out-of-sync documentation. The same happens with code comments and manually written documentation.

I would like to make the case for automated testing of API implementations against a formal spec.

1. Having a spec is the best way to resolve disputes in a flat social structure like voluntary open source collaboration or [any company worth working for](http://www.businessweek.com/articles/2012-04-27/why-there-are-no-bosses-at-valve).
1. Creating a spec is a separate process entirely from creating implementations of that spec. It involves much more than just the backend and database engineers. Leaving it to them practically ensures that idiosyncrasies and bugs will become de-facto standards.
1. With a spec in place, development of API providers and consumers can happen in parallel. This greatly speeds up development.
1. Having a spec allows you to migrate between API providers, or build new API consumers independently. This decoupling is key when scaling apps, hiring talented developers across language barriers, or integrating with other applications.
1. Tooling for RESTful API testing is very limited at this time. But I predict that in the coming months/years we will see an explosion in developer tools and frameworks to test, mock, validate, and analyse API providers.