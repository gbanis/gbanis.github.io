---
layout:     post
title:      "Git with Teams: A Sample Git Workflow"
description:  "A collection of usefull Git commands and workflows for when you collaborate with a team."
date:       2014-11-08 12:00:00
tags:
  - git
  - github
  - heroku
author:     George Banis
---

Here's one of the many ways you can use Git and GitHub to manage your team project.

## Typical Scenario

### Setting Up The Repos
- Admin creates a **new organization** on Github
- Admin creates the `upstream` repo with a readme file
- Team members `fork` the repo (this is the individual team member's `origin` repo)
- Team members `$ git clone <url>` the repo
- Team members `$ git remote add upstream <url>` the origin repo

### Pushing from A Local Branch to A Different Remote Branch

`$ git push origin <local-branch>:<remote-branch>`

### Making Changes & Submitting Updates

- Create a new branch `$ git checkout -b new_feature`
- Make updates
- `$ git add .`
- `$ git commit -m "Issue #: Issue Description"`
- `$ git push origin new_features`
- Make a `pull request` on GitHub
- Admin reviews the code and `merge` the pull request
- All team members `$ git pull upstream master`

> Before anyone's `pull request` can be accepted, it must have the latest accepted code from the `upstream` repo.

### Updating your Fork

- Fetch changes: `$ git fetch upstream`
- Merge them into master: `$ git merge upstream/master master`
- Push them to your remote: `$ git push origin master`

## Other Scenarios

### Git Pull VS. Git Merge

`git pull` = `git fetch` + `git merge`

### Working with Remote Branches

- Create and checkout local branch `$ git checkout -b branch_name`
- Link to remote branch `$ git push origin branch_name`
- Make updates, `stage` and `commit` them
- Push them to remote branch `$ git push`
- Other team member pulls `$ git pull`
- List all remote branches `$ git branch -r`
- Checkout the branch `$ git checkout branch_name`
- Make changes, `stage`, `commit`, and `push` them
- List remote branches `$ git remote show origin`
- Delete the remote branch `$ git push origin :branch_name`
- Delete the local branch `$ git branch -d branch_name`
  - or force delete `$ git branch -D branch_name`
- Clean up all stale remote branches referenced locally `$ git remote prune origin`
- To see all remote info (branches, etc.) `$ git remote show origin`


### Deploying a Branch to Heroku Staging Server

- Create a heroku staging environment `$ heroku create --remote heroku-staging`
- Deploy the `test_branch`: `$ git push heroku-staging test_branch:master`

> Don't forget: You'll need to run all the necessary `rake` commands and install the `env variables` anew on the staging server. To do that you'll need to append `--remote heroku-staging` after each of those commands.

### Tagging and Release Versioning

- To list tags `$ git tag`
- To go back to that tag `$ git checkout <tagname>`
- To add new tag `$ git tag -a v0.0.1 -m "Version 0.0.1"`
- To push tags to remote repo `$ git push --tags`

### Rebasing

#### Initial State

```ruby
# `master` has commited updates
# `branch` also has commited updates

           |  |
master ->  X  Y <- branch
           |  |
           X  Y
           | /
           O
```

#### Run Rebase

Checkout branch and run rebase:

`$ git checkout branch`

`$ git rebase master`

Which does the following:

```ruby
# existing commits are moved to temp location

# then, `master` commits are moved to the current branch

X  X    Y
|  |    |
X  X    Y
| /
O

# `branch` commits are moved on top of `master` commits

   Y
   |
   Y
   |
X  X
|  |
X  X
| /
O

```

#### Merge Updated Branch with Master

Checkout master and merge the branch with it:

`$ git checkout master`

`$ git merge branch`

Which does the following:

```ruby

Y  Y
|  |
Y  Y
|  |
X  X
|  |
X  X
| /
O

```

You can then add a `tag` and/or `delete` the branch.


### Simple Fetch and Rebase Workflow

To update your local `master` with the remote `origin/master`:

- `$ git fetch`
- `$ git rebase`
- If you have conflicts, you can
  - Resolve them in the editor and run `$ git rebase --continue`
  - Skip the patch `$ git rebase --skip`
  - Abort rebasing `$ git rebase --abort`

### Squashing Unwanted Commits with Rebase

> **Caution**: only do this on commits that haven't been pushed to an external/online repo.

`$ git rebase -i <after-this-SHA>` (for interactive pick/squash)

or

`$ git reset --soft <after-this-SHA>` (to remove commit messages)

### Cherry Picking

#### Why Use Cherry Picking

To *selectively* bring the functionality of a single commit to your working branch from another branch, when that commit is between other commits and you dont want to merge/rebase the entire branch to your working branch.

#### How to Use Cherry Pick

`$ git cherry-pick <commit-we-want-SHA>`

To cherry pick and **edit the commit msg**:

`$ git cherry-pick --edit <commit-we-want-SHA>`

To cherry pick **multiple commits** and without committing:

```
$ git cherry-pick --no-commit <SHA-1> <SHA-2>
$ git add .
$ git commit -m "Message"
```

To cherry pick a public/online commit and keep the original commit's info (SHA/Auhtor):

`$ git cherry-pick --signoff <SHA>`



