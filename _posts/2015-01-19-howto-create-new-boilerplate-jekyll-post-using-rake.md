---
layout:     post
title:      "How To Create a New Boilerplate Jekyll Post Using Rake"
description:  "Tired of manually creating files and setting the default params? Here's a Rake task that will do it for you!"
date:       2015-01-19 12:00:00
tags:
  - jekyll
  - rake
author:     George Banis
---

## TL;DR

In this post to learn how to create a new Jekyll post with boilerplate `yaml` info using Rake. That way you won't have to navigate around, create files, and copy-paste info from older posts.

## Building the Rakefile Task

If want, you can jump directly to the [complete rakefile task](#the-complete-rakefile-task) or keep reading to learn how I put it together and how it works.

### Specifications

Here are the things I'd like rake task to do:

1. Ask for a Blog post title
1. Create the file name and post slug
1. Write some boilerplate `yaml` information in the file
1. Save it in the `_drafts` folder

Let's start simple and create a namespace with a description and an empty task:

```ruby
namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    # ...
  end
end
```

Now, let's add a prompt and save the user input:

```ruby
namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    STDOUT.print "Enter post name:\n> "
    post_name = STDIN.gets.chomp
    # ...
  end
end
```

Excellent! Now, we need to do some string manipulation to create the post for the slug.

We will need to:

- downcase all words,
- remove all escape words (because Google doesn't index them),
- remove spaces and special characters,
- and separate all words with dashes

```ruby
STOPWORDS = %w(i a about an are as at be by com for from how in is it of on or that the this to was what when where who will with the www)

namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    STDOUT.print "Enter post name:\n> "
    post_name = STDIN.gets.chomp

    post_slug = post_name
      .gsub(/[^a-zA-Z ]/,'').gsub(/ +/,' ') # remove special characters and double spaces
      .split(" ")
      .map { |word| word.downcase }
      .keep_if { |word| !STOPWORDS.index(word) } # keep if the word is not a stop word
      .join("-")

    # ...
  end
end
```

Next, we need to get the `current_date` which we will use in the filename and in the `yaml` info.

With the `current_date` and the `post_slug`, setting the `file_path` is pretty simple.

Note: When I create new posts I put them in my `_drafts` folder. If you want to put them directly in your `_posts` folder, just set:

`file_path = "_posts/#{current_date}-#{post_slug}.md"`

```ruby
STOPWORDS = %w(i a about an are as at be by com for from how in is it of on or that the this to was what when where who will with the www)

namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    STDOUT.print "Enter post name:\n> "
    post_name = STDIN.gets.chomp
    post_slug = post_name
      .gsub(/how to/i, "howto")
      .gsub(/[^a-zA-Z ]/,'').gsub(/ +/,' ')
      .split(" ")
      .map { |word| word.downcase }
      .keep_if { |word| !STOPWORDS.index(word) }
      .join("-")
    current_date = Time.now.getutc.strftime("%Y-%m-%d") # YYYY-MM-DD
    file_path = "_drafts/#{current_date}-#{post_slug}.md"

  # ...
  end
end
```

Alright, almost there! The last thing to do is to actually create the file with a `system` `echo` command. Check it out below, along with the complete task.

## The Complete Rakefile Task

```ruby
STOPWORDS = %w(i a about an are as at be by com for from how in is it of on or that the this to was what when where who will with the www)

namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    STDOUT.print "Enter post name:\n> "
    post_name = STDIN.gets.chomp
    post_slug = post_name
      .gsub(/how to/i, "howto")
      .gsub(/[^a-zA-Z ]/,'').gsub(/ +/,' ')
      .split(" ")
      .map { |word| word.downcase }
      .keep_if { |word| !STOPWORDS.index(word) }
      .join("-")
    current_date = Time.now.getutc.strftime("%Y-%m-%d")
    file_path = "_drafts/#{current_date}-#{post_slug}.md"
  `echo '---
layout:     post
title:      "#{post_name}"
date:       #{current_date} 12:00:00
tags:
  - tag
  - tag
author:     George Banis
---' > #{file_path}`
  end
end
```

## Bonus

Here's another quick tip to make your life even easier!

Just create an alias to your Jekyll folder to get there even faster.

- First, open `~/.bash_profile` with your favorite editor
- Add an alias to your Jekyll folder `alias cdjk='cd ~/path-to-jekyll-folder'`

Just enter `cdjk` in your terminal to `cd` in your Jekyll folder.

Now you have no excuses not to blog! :-)

**Have a question? [Leave a comment!](#comments)**
