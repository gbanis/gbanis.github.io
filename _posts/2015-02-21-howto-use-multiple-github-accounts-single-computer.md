---
layout:       post
title:        "How To Use Multiple Github Accounts On A Single Computer"
description:  "The easy steps to set up another Github account on your computer and use both seamlessly."
date:         2015-02-21 12:00:00
tags:
  - github
  - git
  - ssh
author:       George Banis
---

## TL;DR

This post will show you how to set up your computer to use multiple Github accounts on a single computer.

## Getting Started

I am assuming you already have a Github account setup properly on your machine. For the sake of this demo let's call it `default-acc`.

## Set up an SSH Key

First we'll need to create and register a new SSH key.

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "your-key-name"
```

I like to name my keys in such way so I don't forget what they are about. For this demo let's just name it `second-github-account`.

Next, add the key to SSH:

```
$ ssh-add ~/.ssh/second-github-account
```

## Associate the New Key with Github

Now you need to associate this key with Github:

- Go to [Github > Settings > SSH keys](https://github.com/settings/ssh).
- Add SSH key
- Give it a title. I'd name it `<computer name> <key name>`. For example `Macbook Air second-github-account`.
- Paste in the **public** key. You can find this by typing `cat ~/.ssh/second-github-account.pub` in your terminal. (Don't forget the `.pub` extension.)

## Create a Config File

In order for Git to be able to distinguish which key it should use for each repo, you'll need to setup a `config` file.

```
$ touch ~/.ssh/config
$ subl ~/.ssh/config
```

In there you'll need to add your current/default account and the new one we just created:

```
#Default GitHub Account
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/first-github-account

#Second GitHub Account
Host github-second
  HostName github.com
  User git
  IdentityFile ~/.ssh/second-github-account
```

## Using the New Github Account with Git

Now, you'll use git pretty much the same way you are used to. The only thing that is different is the way you refer to your remote repo.

For example, if the SSH link Github provides you is `git@github.com:user-name/repo-name.git` you'll need to change it to `git@github-second:user-name/repo-name.git`.

Where did `git@github-second` come from?

Check out the config file we just created. Under the second account we have `Host github-second` and `User git`. That's it!

Now, when you clone a repo or add a remote, the only thing you need to remember is to change `github.com` to your `github-second` host.

`$ git@github.com:<username>/<repo>.git` becomes `$ git clone git@github-second:<username>/<repo>.git`

`$ git remote add origin git@github.com:<username>/<repo>.git` becomes `$ git remote add origin git@github-second:<username>/<repo>.git`