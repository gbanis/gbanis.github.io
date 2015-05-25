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
