# To publish:

# $ jkb # => build the website
# $ gaa
# $ gcm "Message"
# $ rake

require "rubygems"
require "tmpdir"
require "bundler/setup"
require "jekyll"

GITHUB_REPONAME = "gbanis/gbanis.github.io"
STOPWORDS = %w(i a about an are as at be by com for from how in is it of on or that the this to was what when where who will with the www)


desc "Build and push to origin/source master"
task :push do
  system "git push origin master:refs/heads/source --force"
end

namespace :post do
  desc "Creates a new blog post in _drafts folder"
  task :new do
    STDOUT.print "Enter post name:\n> "
    post_name = STDIN.gets.chomp
    post_slug = post_name
      .gsub(/how to/i, "howto")
      .gsub(/[^a-zA-Z ]/,'').gsub(/ +/,' ') # remove special characters and double spaces
      .split(" ")
      .map { |word| word.downcase }
      .keep_if { |word| !STOPWORDS.index(word) }
      .join("-")
    current_date = Time.now.getutc.strftime("%Y-%m-%d")
    file_path = "_drafts/#{current_date}-#{post_slug}.md"
  `echo '---
layout:       post
title:        "#{post_name}"
description:  "Enter description"
date:         #{current_date} 12:00:00
tags:
  - enter tag
  - enter tag
author:       George Banis
---' > #{file_path}`
  end
end

namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "_site"
    })).process
  end

  desc "Generate and publish: blog to master branch, source files to source branch"
  task :publish => [:generate] do
    # Commit your site files on master branch
    Dir.mktmpdir do |tmp|

      # Create temporary dir with _site files
      cp_r "_site/.", tmp

      # Save current dir
      pwd = Dir.pwd

      # Change to temp _site directory and commit to Github master with current timestamp
      Dir.chdir tmp
      system "git init"
      system "git add ."
      message = "Site updated at #{Time.now.utc}"
      system "git commit -m #{message.inspect}"
      # system "git remote add origin git@github-:#{GITHUB_REPONAME}.git"
      system "git remote add origin git@github-gbanis:#{GITHUB_REPONAME}.git"
      system "git push origin master:refs/heads/master --force"

      # Change back to the original directory
      Dir.chdir pwd
    end
  end
end

task :default => ["site:publish"]

# Inspired by: http://ixti.net/software/2013/01/28/using-jekyll-plugins-on-github-pages.html
