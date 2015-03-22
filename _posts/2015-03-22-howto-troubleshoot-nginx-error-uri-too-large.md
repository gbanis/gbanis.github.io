---
layout:       post
title:        "How to Troubleshoot Nginx Error 414: URI Too Large"
description:  "Find out how to configure Nginx and stop getting 414 Errors for AJAX calls with large headers."
date:         2015-03-22 12:00:00
tags:
  - nginx
  - ajax
author:       George Banis
---

While large AJAX requests could be considered a smell, sometimes it might be easier to simply configure nginx so that it handles larger requests than simply slicing the request to and sending a batch of smaller ones.

## Why Does This Error Occur?

As I was developing on my local dev environment, everything seemed to work fine. But then when I deployed my code on an Amazon EC2 instance, Nginx started complaining sending 414 "URI Too Large" errors.

The most probable explanation is that the local web server that I was using (Thin/Webrick/Unicorn) by default accepts larger AJAX requests. On the other hand, Nginx usually defaults to 4kb or 8kb max.

## Should I Change Nginx's Configuration?

Ideally, you would want to trim your requests, or split them into a batch and send them via AJAX concurrently, rather than sending one giant request.

However, this might not be feasible sometimes. Maybe a 3rd party JavaScript library sends the request. Or the request looses context if it gets split.

For those occasions, bumping up the request cap might be the only option.

## How to Increase Nginx's Cap for Header Size?

First, locate `nginx.conf`. It's usually found at `/etc/nginx/nginx.conf`.

Then, add the following header in the `server` or `http` and `https` part of the file:

```
large_client_header_buffers 4 16k
```

Note:

- You may want to further increase the allowed header size from 16k.
- This header might already exist. If it does, just edit it.

Once you are done, make sure you save the configuration file and restart nginx.

Some ways to restart nginx:

- `service nginx restart`
- `nginx -s stop` then `nginx`
- `/usr/sbin/nginx -c /etc/nginx/nginx.conf`

Hopefully you should be all set!

But in case you are still having trouble, [let me know in the comments](#comments) and I'd love to help you with troubleshooting.