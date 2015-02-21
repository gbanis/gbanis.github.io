---
layout:     post
title:      "Rails and Ruby Optimization"
description:  "Find out some of the essensial things you can do to optimize your Rails application."
date:       2014-11-21 12:00:00
tags:
  - ruby
  - ruby on rails
  - optimization
  - n+1 queries
author:     George Banis
---

## Objectives

1. Fix N+1 queries
1. Refactor using best practices (code smells)
1. Profile Rails in production (using New Relic)

## N+1 Queries

### ActiveRecord Query Optimization

[ActiveRecord Query Optimization on GitHub](https://github.com/ga-wdi-boston/wdi_10_rails_query_optimization)

Eager: lets go and do all things *upfront* and figure out all the answers *upfront*
Lazy: wait (don't compute) until we need something to go and get it

> `ActiveRecord Queries` is lazy by default (do not eagerly load)

### Counter Caching

Placing an integer counter field / column on a Model that acts as a counter. For example a counter for the amount of comments a post has. That saves us from making multiple queries to find that amount.

To see the end of a file:

`tail -f log/bullet.log`

#### To Reset Counters

You can run a command in `rails console`

`Article.all.each {|article| Article.reset_counters(article.id, :comments)}`

Or you can create a rake task:

`$ rails g task articles reset_counter_cache`

Then, add the code above in `lib/tasks/articles.rake` like so:

```ruby
namespace :articles do
  desc "Reset the articles counter cache"
  task reset_counter_cache: :environment do
    Article.all.each {|article| Article.reset_counters(article.id, :comments)}
    puts "--> Articles counter cache was successfully reset."
  end
end
```

FYI: To run `bash` commands with rake, just include the command in backticks

## New Relic on Heroku

### To install

- On an existing heroku app, run: `heroku addons:add newrelic:stark`
- Add: `gem 'newrelic_rpm'` to your `Gemfile`
- Run: `$ bundle`
- Run: `$ curl https://gist.githubusercontent.com/rwdaigle/2253296/raw/newrelic.yml > config/newrelic.yml`
- Run: `$ heroku config:set NEW_RELIC_APP_NAME="PICK A NAME AND PUT IT HERE"`

Then go to your Heroku app that has New Relic installed and click on the link.

You may need to add a key in your `ENV`, `.gitignore` and add in the `yml` file.

More info here on New Relic documentation and installation [here](https://devcenter.heroku.com/articles/newrelic#add-on-installation).

## Refactoring

### What Can We Refactor in Ruby/Rails?

Code smells:

- Long methods
- Logic in views
- Too much in controller / too little in model
- Repetitive code
- Multiple nested if/else statements
- Lack of constants
- Dead code (code no longer being used)
- Multiple methods doing same thing
- Poor error handling
- Large classes (look at [God Object](http://en.wikipedia.org/wiki/God_object))
- Need less 'complexity'
- Highly coupled classes (that have many dependencies to other classes)
- Feature envy (class using methods of other class exessively)
- Immobile code (code marked "don't touch")
- Nested iterrators (blocks inside blocks)
- Sibling classes (similar external/internal variations)
- Lazy/freeloader class (a class that doesn't do much)
- Too many variables
- Message chains
- Macaroni code (mixing multiple languages in same document)
- Inappropriate intimacy (class that has dependencies on internal implementation of another class)
- Primitive obsession (using primitives to represent other things, eg. date, price, etc.)

Other:

- Indentation
- Poor naming
- Poor comments
- Storing API keys outside `.env` file

### Refactoring Patterns

Read more about refactoring [here](https://github.com/ga-wdi-boston/wdi_10_design_patterns_refactoring)





