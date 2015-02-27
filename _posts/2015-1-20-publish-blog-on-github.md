---
layout: post
title: Publishing a blog on GitHub
description: Provides a quick start for setting up a website on GitHub using Jekyll. Also provides information on running Jekyll locally.
---

I have been looking for a simple and minimalistic approach to publishing a blog.
I believe I have found it: [GitHub Pages](https://pages.github.com){:target="_blank"}

##Quick Start

1. Create an account on [GitHub](https://github.com){:target="_blank"}
2. Create a new GitHub repository by forking [http://www.github.com/barryclark/jekyll-now](http://www.github.com/barryclark/jekyll-now){:target="_blank"}. Rename the new repository to `yourusername.github.io`
3. Now go to your browser and type this address: `http://yourusername.github.io`
4. Voila! You have a website.

##What just happened?

- Every GitHub account gets a website. Default URL: `http://yourusername.github.io`
- The source of this website is a Git repository: `https://github.com/yourusername/yourusername.github.io.git`
- The contents of this Git repository are processed by [Jekyll](http://jekyllrb.com){:target="_blank"}, which generates a set of static HTML files that are served by the web server of GitHub.
- When you forked [Jekyll Now](http://www.github.com/barryclark/jekyll-now){:target="_blank"}, you made a copy of an existing repository ([Jekyll Now](http://www.github.com/barryclark/jekyll-now){:target="_blank"} makes it easier to create a Jekyll blog by eliminating a lot of the upfront setup).

##Customize your home page

1. Clone your GitHub repository `https://github.com/yourusername/yourusername.github.io.git` to your local machine.
2. Edit the `_config.yml` file (change the name and description), commit, and push to GitHub.
3. Now go to your browser again and refresh `http://yourusername.github.io`. The name and description have changed.

##Start blogging

1. Create a new post by creating a file in the `_posts` directory and name it according to this format: **YEAR-MONTH-DAY-title.md**
2. Copy the first four lines from the `_posts/2014-3-3-Hello-World.md` file and change the title.
3. Put in the contents. Commit and push to GitHub.
4. Go to your browser and refresh your home page. Your new post will appear.

From this point on, all you need to do is add new posts in the `_posts` directory.
You might also want to learn [Markdown](https://help.github.com/articles/markdown-basics){:target="_blank"}, which is a lightweight markup language
that uses a simple formatting syntax so that it can be easily converted to HTML.

##The gory details

Jekyll Now provides a nice way to get started without being too technical. But there is a good chance you want to change the layout
and preview the changes on your local machine before publishing them on the internet. Moreover, you want to preview
your changes as they would appear on GitHub Pages. Below are the steps I had to go through
on my machine (Ubuntu 14.10):

###Prerequisites

The prerequisites ensure the successful installation of Jekyll. Here are the commands:

~~~
sudo apt-get install build-essential
sudo apt-get install ruby2.1
sudo apt-get install ruby-dev
sudo apt-get install ruby-execjs
sudo apt-get install zlib1g-dev
sudo gem install bundler
~~~

###Setup
In the Git repository, create a file called Gemfile with these two lines:

~~~
source 'https://rubygems.org'
gem 'github-pages'
~~~

###Install Jekyll
In the Git repository, type this command:

~~~
bundle install
~~~

###Publish your website locally
In the Git repository, type this command:

~~~
bundle exec jekyll serve
~~~

###View your website locally

~~~
Point your browser to http://localhost:4000
~~~

###Writing and publishing drafts

Put your drafts in the `_drafts` directory. Then type this command:

~~~
bundle exec jekyll serve --drafts
~~~


References:

- [http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages](http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages){:target="_blank"}
