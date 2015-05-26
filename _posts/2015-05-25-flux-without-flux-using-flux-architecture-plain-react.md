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

In this post I'll share with you a design pattern that implements a very similar Flux architecture, but using just plain React.

I tested this "Flux without Flux" pattern building *large front-end applications* as well as *reusable components* with great success and thought I'd share it with you.

## To Flux or not to Flux?

This is a very common dilemma among engineers that use React &mdash; and rightfully so.

You want to keep your code better organized so that it is easier to reason about and allow the rest of your team to quickly understand how your application is structured.

On the other hand, you looked at the [online Flux documentation](https://facebook.github.io/flux/docs/overview.html), maybe tried to walk through some of the [Flux examples](https://github.com/facebook/flux/tree/master/examples), and started having second thoughts:

- It looks more complicated than just using React
- Will my team be able to pick it up quickly?
- Which [Flux flavor](https://reactjsnews.com/the-state-of-flux/) should I pick?

Sound familiar?

Well, don't despair!

The "Flux without Flux" pattern that I'm about to share will give you many of the benefits of Flux architecture while using vanilla React.

> Disclaimer: I have not used Flux extensively so I can't speak about how this pattern compares with it or in which cases you might need to use the actual Flux architecture over the "Flux without Flux" patter that I am suggesting. Please use your own judgment. This might not be the perfect solution to all your problems.

## A Quick Flux Recap

If you visited Facebook's [Flux documentation](https://facebook.github.io/flux/docs/overview.html#structure-and-data-flow) you probably came across this chart:

![Flux data flow](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

The basic premise of the Flux architecture is:

- An `action` is triggered which creates a `payload` and hands it to the `dispatcher`
- The `dispatcher` delegates the `payload` to all the `stores` registered with it
- The `stores` update themselves and emit a change event
- The React `views` listen to the change events and update themselves based on the new data from the stores

## The Flux without Flux Pattern

In the FWF pattern, the Flux architecture is adapted to use React elements:

`Event => onAction => State => View`

- A user interacts with your application and generates an `event`
- The event handler creates a `payload` and hands it to the parent component's `onAction`
- The `onAction` calls the appropriate method that updates the `state`
- React automatically updates all components passing new `props` to them

Not 100% sure how to piece this together?

In the parent component create a callback method (I like to call it `onAction`) and pass it down to all your child components via `props`.

When an event gets triggered, call the appropriate child component's event handler. In it, create a `payload` object with an `action` and a `value`. Then, call the parent's `onAction`, that is passed down via `props`, giving it the `payload` you created.

Finally, in the parent component use a `switch` that reads the `payload.action` and delegates it to the appropriate method that knows how to update the `state` with the `payload.value`.

React takes care of the rest automatically.

## A Simple "Flux without Flux" Example

Here's an example of the "Flux without Flux" pattern:

<iframe width="100%" height="400" src="//jsfiddle.net/gbanis/zve3xngL/3/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

In this example I've created a simple application allows us to create and list `comments`.

Let's walk through this example:

- Clicking the `submit` button in `NewComment` uses `this.props.onAction` to dispatch an action with `payload`:

  ```
  payload = {
    action: "SUBMIT_NEW_COMMENT"
    comment: {
      comment: this.state.comment,
      author: this.state.author
    }
  }
  ```

- `onAction` grabs the `payload`, runs it through the `switch` and calls `_onSubmitNewComment`
- `_onSubmitNewComment` updates the state of the parent component, `CommentApp`
- `CommentApp` passes the new `comments` from its `state` down to `CommentList` via `props`
- `CommentList` updates, rendering the new list of `comments`

## Final Thoughts

I've started using the "Flux without Flux" pattern recently and so far it feels very nice and effortless. Things are where my team and I expect them to be and it's quite easy to navigate around the codebase.

I also find it easy to reason about events and their consequences. Plus, I can just use the React I know and love!

That being said, this is a fairly new pattern and React is not that old either. So please take my suggestions with a grain of salt and if you decide to give this a try, I'd love to hear about your experience!
