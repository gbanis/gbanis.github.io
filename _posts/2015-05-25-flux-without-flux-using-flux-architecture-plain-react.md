---
layout:       post
title:        "Flux without Flux: Using Flux Architecture in Plain React"
description:  "How to organize React in a way that uses the Flux architecture without a Flux library."
date:         2015-05-25 12:00:00
tags:
  - ReactJS
  - Flux
author:       George Banis
---

## TL;DR

If you are into ReactJS you probably wondered whether or not you should use Flux.

In this post I'll share with you a design pattern that implements the same `Action => Dispatcher => Store => View` architecture using plain React.

I tested this "Flux without Flux" pattern building *large front-end applications* as well as *reusable components* with great success and thought I'd share it with you.

## To Flux or not to Flux?

This is a very common dilemma among React engineers &mdash; and rightfully so.

You want to keep your code better organized so that it is easier to reason about and allow the rest of your team to quickly understand how your application is structured.

On the other hand, you looked at the [online Flux documentation](https://facebook.github.io/flux/docs/overview.html), maybe tried to walk through some of the [Flux examples](https://github.com/facebook/flux/tree/master/examples), and started having second thoughts:

- It's more complicated than just using React
- Will my team be able to pick it up quickly?
- Which [Flux flavor](https://reactjsnews.com/the-state-of-flux/) should I pick?

Sound familiar?

Well, don't despair!

The "Flux without Flux" pattern that I'm about to share will give you many of the benefits of Flux architecture while using vanilla React.

> Disclaimer: I have not used Flux extensively so I can't speak about how this pattern compares with it or in which cases you might need to use the actual Flux architecture over the "Flux without Flux" patter that I am suggesting.

## The "Flux without Flux" Pattern

The basic idea is that you create a callback method in the parent component (I like to call it `onAction`) and pass it down to all your child components.

In your child components, when you want to trigger an action on the parent, you create a `payload` with an `action` and a `value`. Then you call the parent's `onAction` that is passed down via `props` passing it the `payload` you created.

Finally, in the parent component you have a `switch` that reads the `payload.action` and delegates it to the appropriate method.

## Data Flow in "Flux without Flux"

If you visited Facebook's [Flux documentation](https://facebook.github.io/flux/docs/overview.html#structure-and-data-flow) you probably came across this chart:

![Flux data flow](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

Yup, you guessed right. The "Flux without Flux" pattern uses the same chart for data flow:

- As our `Dispatcher` we use the `onAction` callback that is passed from the parent to the child components
- For `Store` we use React's `state`
- The `View` is our React child component
- And finally, `Action` is any event in the child component that calls the parent's `onAction` with a `payload`

## A "Flux without Flux" Example

## Final Thoughts

I've started using the "Flux without Flux" pattern recently and so far it feels very nice and effortless. Things are where my team and I expect them to be and it's quite easy to navigate around my codebase.

I also find it easy to reason about events and their consequences. Plus, I can just use the React I know and love!

That being said, this is a fairly new pattern for a fairly new front-end library. So please take my suggestions with a grain of salt.

And if you decide to give this a try, I'd love to hear about your experience!
