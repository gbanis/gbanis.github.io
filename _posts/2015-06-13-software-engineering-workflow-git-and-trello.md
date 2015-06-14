---
layout:       post
title:        "Software Engineering Workflow with Git and Trello"
description:  "A workflow using Git and Trello to help you build awesome applications without worrying about project management."
date:         2015-06-13 12:00:00
tags:
  - git
  - trello
  - workflow
  - software engineering
author:       George Banis
---

If you are reading this post, you are probably looking for a *comprehensive* and *easy to follow* workflow.

So, I decided to share the workflow that I use for my side-projects and for this blog! It helps me stay focused, continue being productive, and track my progress.

It is a variation of the popular [Kanban workflow](https://en.wikipedia.org/wiki/Kanban_(development)#Kanban_board_example), an Agile and Lean workflow which many teams and individuals use successfully.

*If you are so inclined, you can also read more about [Open Kanban](https://github.com/agilelion/Open-Kanban), an open-source flavor of the Kanban methodology.*

## Slides

If you just want the summary, take a quick peak through the slides.

<div class="iframe-container">
  <iframe src="//slides.com/gbanis/software-development-workflow-git-trello/embed?style=light" width="576" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
</div>


## Tools of the Trade

The tools we'll be using are:

- [Trello](https://trello.com) to add new stories or bugs and manage our project
- [Git](https://git-scm.com/) to version our code
- [Github](https://github.com) to save our project

*I am intentionally skipping any mention of deployment tools. There are many great options and of course they all depend on your stack. Feel free to check out [Codeship's ebook](http://codeship.io.s3.amazonaws.com/ebooks/Codeship_Efficiency_in_Development_Workflows.pdf) &mdash; It's a great read.*

## Trello Lists

Here are the lists I like to use for my Trello boards:

1. `Backlog`
1. `Next Up`
1. `In Development`
1. `In Code Review`
1. `To Release`
1. `Live`
1. `Abandoned`

Feel free to copy this [sample board](https://trello.com/b/aKD2qNE3).

## The Workflow

Let's dive into the details of the workflow and how to use each list.

### 1. Backlog

When you decide to add new features or improved existing ones, you'll be creating new cards and adding them to the `Backlog`.

The `Backlog` is also the right place to add bugs that you discover or your customers report.

It usually helps to generally arrange the `Backlog` cards by order of importance. This way you will have at least a rough idea of what you want to start working next.

Once a day, sift through the `Backlog` and pick the stories you want to work on next and add them to `Next Up`.

### 2. Next Up

`Next Up` contains all the stories that you want to implement next. You want to have about 2-3 (but not more than 5) stories per team member in this list.

This is separate from the `Backlog` because you want to be able to quickly glance on what should be developed.

It will help you avoid decision paralysis and quickly start working on the next thing that needs to be developed.

You should move stories from the `Backlog` to `Next Up` once a day or once every few days to keep your queue full and not waste time deciding what to work on next.

### 3. In Development

So far in the workflow we mostly used Trello. Now it's time to start working on our codebase:

1. Move a card from `Next Up` to `In Development`
1. Create a new branch on your repo with the name of the story
  - `git checkout -b <branch-name>`
1. Work on the story
1. If you have a test suite, run your tests
1. When you are done with your story move it to `Code Review` and issue a `pull request` on Github
1. Inform your team
1. Pick up the next story from `Next Up` and repeat

**Important!**

Make sure you merge `master` whenever an other branch gets merged into `master`:

```bash
$ git checkout master
$ git pull
$ git checkout <your-branch>
$ git merge master
```

If you have merge conflicts:

```bash
// Open the files with conflicts
// Coordinate with your team members to resolve the conflicts
$ git add .
$ git commit
```

### 4. In Code Review

> If you are skeptical about the `Code Review` process, watch [this talk from RailsConf](https://www.youtube.com/watch?v=PJjmw9TRB7s). I am a big supporter of code reviewing because: 1) it is a great way to learn from others, 2) it helps your team be more aware of your codebase, and of course 3) it results in an overall healthier codebase.

If you are working with a team have one of your team members review the open pull request on Github.

If you are flying solo, do the CR yourself but be very religious about the process. Try to look at your code as if someone else has written it.

Once that's done, merge your pull request into `master`. If you keep your branch fresh with the latest `master` you shouldn't have any merge conflicts. But even if you do, just go ahead and resolve them.

Finally, delete the branch and move the story's card to `To Release`.

### 5. To Release

You made it so far! That's awesome!

Now, it's time to start the relase process and get your story out to the world.

Depending on the situation you may want to:

- Put `master` on a staging box and give it a spin
- Deploy to production
- Run your build suite and deploy to production
- Run your continuous delivery suite and deploy to production
- Wait for other complementary stories to release them all together

### 6. Live

Congratulations! Time your give yourself a quick pat in the back.

But don't celebrate for too long &mdash; another story awaits in `Next Up` :-)

### 7. Abandoned

Not every story in the `Backlog` will get released. Maybe you filed a duplicate story. Or maybe you decided not to implement a feature. The `Abandoned` list is to collect all those dormant stories so you can keep them filed instead of just deleting them.

## Final Thoughts

I've been using this workflow (or a similar one) in most of the teams I've worked on and it seems to be working pretty well for me.

Have you used it yourself? Do you have any feedback? Or maybe you have a better one to recommend?

Feel free to leave a [comment](#comments).
