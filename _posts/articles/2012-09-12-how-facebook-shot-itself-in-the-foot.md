---
title: How Facebook Shot Itself in the Foot
abstract: The Facebook iOS app shifted from HTML5 to native code.
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---

Facebook's new iOS mobile app ditched HTML5 and switched to Objective C, i.e. native code. Supposedly this provides a better user experience because of improved "performance."

I think this is short-sighted and Facebook users will be worse off in the long run.

So if you're a developer considering HTML5 or native code for your next mobile app, please consider the following.

## The Web is the Future of Technology

We are in the midst of a frontend renaissance. A flood of new frameworks and tools are increasing frontend developer productivity. Software quality is improving by leaps and bounds. Meanwhile, native app tools are created by one company per platform and are stuck in the '90s.

Sure, Facebook has unlimited resources but they also live-or-die by automation provided through tools. Their mobile team now has to work with inferior tools and will be at a disadvantage to competitors leveraging the web platform.

## Facebook is a Web Engineering Company

Facebook's engineering culture is successful because they have some of the best tooling in the industry. All their tools are focused on web technology. Switching this to native app technology will be painful at best, and impossible to replicate in many cases.

App stores break modern best practices like continuous delivery, A/B testing, gradual rollouts, bucket testing, etc.

Without these tools it will become much more difficult to create quality products.

## The Old Facebook iOS App Was Not a Modern Mobile App

Calling the old Facebook iOS app an HTML5 app is like calling GMail a Javascript app. (*Hint:* GMail is built in Java and transpiled to bloated JavaScript using the Google Web Toolkit abomination. *shudder*)

The old Facebook iOS app was built *on top of* the desktop website. So, instead of building a light-weight HTML5 app, it actually transferred huge chunks of HTML, CSS, and images that were meant for high-performance desktop browsers.

Contrast this with the mobile HTML5 apps of Twitter, LinkedIn, or Yahoo Mail. Those are responsive one-page-apps which only transfer the minimum necessary data via optimised JSON/HTTP APIs. Their performance is as good as native, if not better, thanks to hardware acceleration and excellent mobile browser UI frameworks.

## Native In-App Advertising is a Dead End

Google and Apple hold the keys of the kingdom. Apple is pushing iAds and Google is pushing Admob onto its app developers. All it takes is their veto to kill any Facebook native in-app advertising plans.

With a web-app no one can stop Facebook from showing its own advertising or finding alternative monetisation methods.

## The Money is in Third Party Site Advertising

What Facebook needs to do is build a Google AdSense competitor. Advertising will earn Google over $40 billion this year. Facebook is projected to earn a paltry $4 billion this year from its on-site advertising.

Facebook has unrivaled audience data from Facebook profiles. It can also track users on sites with a Like or Share button, or that use Connect authentication. It can even extract your Google Search queries this way because the HTTP Referrer header is a snitch.

Google is scared to death of Facebook, and they should be. Google can not even guess a visitor's gender >90% of the time. So they are building Google Plus, which is failing to attract users. If, or when, Facebook's AdSense rolls out, it will do to Google what Google did to Yahoo. I predict that Google Search will feature Facebook ads because Google's own advertising network won't be competitive.

## Web Apps Can Track Users Across Sites. Native Apps Can't.

To monetise mobile users, Facebook needs to have them logged in through the mobile browser. If you're logged in to the Facebook native app, your mobile browser doesn't have a Facebook cookie. So when you're browsing mobile sites, Facebook cannot show relevant advertising.

By pushing users away from the mobile browsers and into a native app, Facebook eliminated the easiest way to monetise its mobile users.