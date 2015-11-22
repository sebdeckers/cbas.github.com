---
title: Front End Workflow
abstract: A step-by-step approach to building any web app.
published: false
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---
## Iterating the Design
Cheap prototyping on paper
## Content Items
Write down as plain text. One line per content item.
## Semantic Structure
Wrap each content item in a meaningful HTML tag or class name.
## CSS Selectors
Create an empty placeholder CSS selector for the HTML tag or class name of each element.
## Dummy Content
Enter fake data that covers the main use case and most edge cases.
## Hide Everything
Add a CSS rule to `display: none;` all the things.
## Gradual Reveal
Starting from the top level, layout and style the elements.
## Pseudo Code
Write plain english in JS comments to describe each step the application takes.
## Tests
Add test cases for each step.
## Implementation
Replace each commented step with a real implementation. Stub out calls to other steps. Add more refined tests to cover edge cases.
