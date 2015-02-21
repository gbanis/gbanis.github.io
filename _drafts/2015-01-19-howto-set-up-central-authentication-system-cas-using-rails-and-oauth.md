---
layout:     post
title:      "How to Set Up a Central Authentication System (CAS) Using Rails and OAuth 2"
date:       2015-01-19 12:00:00
tags:
  - central authentication system
  - cas
  - service oriented architecture
  - soa
  - authentication
author:     George Banis
---

I recently watched [Jeremy Green's](https://twitter.com/jagthedrummer) presentations from the 2014 [RailsConf](http://www.youtube.com/watch?v=L1B_HpCW8bs) and [EmberConf](http://www.youtube.com/watch?v=q_gy8EgN8FE) on Service Oriented Authentication which inspired me to write this post.

## TL;DR

Read this post to find out how to set up a Rails app to be used as a Central Authentication System.

Hopefully, if you follow all the steps you will have:

- A Rails authentication server using OAuth 2
- A simple Rails back-end server that has some public and some protected assets
- A simple JavaScript front-end app that communicates with the back-end server via AJAX

![Service Oriented Architecture Central Authentication System](/assets/soa-cas-architecture.svg)

## First, Setup your Repos

I put together 3 repos:

- one for my back end Rails that will host the business logic
- one for my OAuth CAS server
- one for my front end
