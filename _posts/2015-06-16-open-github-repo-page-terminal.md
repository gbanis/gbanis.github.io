---
layout:       post
title:        "Open Github Repo Page from the Terminal"
description:  "Open the Github page of your current repository using a bash command from your terminal."
date:         2015-06-16 12:00:00
tags:
  - Github
  - terminal
  - bash
author:       George Banis
---

![Open Github repository page from the terminal (gif)](/assets/open-github-from-terminal.gif)

## TL;DR

You are working on your repository and quickly want to quickly open its Github page. So you type `gh` in your terminal and voila!

## The Script to Open Github from your Terminal

To make this work, you need a script that grabs your repo's `remote`, constructs the URL and opens it with your default browser.

This is what the script looks like:

```bash
# ~/.personal-scripts.sh

function gh() {

  # get your repository's remote origin url
  remote=$(git config --local --get remote.origin.url)

  # removes '.git' from the end of the remote
  remote=${remote%.git}

  # exit if there is no repository in the PWD
  if [ "$remote" == "" ]
    then
     echo "Not a git repository or no remote.origin.url set"
     exit 1;
  fi

  # construct the Github url
  url=${remote/git\@github\.com\:/https://github.com/}

  # open the url
  open $url
}
```

## Where Does This Script Go?

First, save the script in a file. I saved it in `~/.personal-scripts.sh`

Then, link it in you `.bash_profile`:

```bash
# ~/.bash_profile
...
source ~/.personal-scripts.sh
```

Finally, reload your terminal window, or close it and open a new one.

The command `gh` will now open the Github repo linked to your PWD.

## How to Change the Command

If you want to use a different command you can simply change the name of the function:

```bash
# from
function gh() {

# to
function preferredname() {
```

## Adjusting this for Multiple Github Accounts

If you are using multiple Github accounts on the same machine, then the above script needs some small tweaking:

```bash
# ~/.personal-scripts.sh

function gh() {
  remote=$(git config --local --get remote.origin.url)
  remote=${remote%.git}
  if [ "$remote" == "" ]
    then
     echo "Not a git repository or no remote.origin.url set"
     exit 1;
  fi

  # get the user email associated with the current repo
  email=$(git config --local --get user.email)

  # use a conditional to construct the correct Github url
  if [ "$email" == "other@user.com" ]
  then

    # get the second account's Github host from ~/.ssh/config
    # for example, if the other account's Github host is `github.com-other`
    url=${remote/git\@github\.com-other\:/https://github.com/}
  else

    # for the default host
    url=${remote/git\@github\.com\:/https://github.com/}
  fi

  # open the url
  open $url
}
```
