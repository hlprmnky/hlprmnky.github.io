---
layout: post
title: Colophon
excerpt: This first blog post details how I set up a blog with org-mode, Jekyll, and Github pages.
---

# {{ page.title }}

I have decided that it is high time to engage in the creation of a blog. I hope to share ideas and practical experiences that are helpful to others. My primary driver, though, is a desire to connect more fully with the amazing community of Clojure hackers and other folks who have so greatly enhanced my own life, by sharing freely of their awesomeness.

My hope is that maintaining a blog will imbue this vague desire with some discipline, and that I will start taking the time and effort to engage in venues where blog posts might be read by others.

The first post-worthy adventure I have found myself on is actually setting myself up with a blog that is easy enough for me to maintain that I might <span class="underline">actually do it</span>. I ended up using a combination of:

-   Github Pages for hosting
-   Org-mode and org-export-md to produce content, and
-   A local Jekyll install running in auto-refresh mode for content development and testing

It has taken some work, but now that it's set up, I find it super-easy and fun to use. In this post, I will detail how I got it that way.

# Using Github Pages and Jekyll to serve up Markdown

There is some documentation at [Github's Pages site](http://github.io/), however after a bunch of trial and error I found [this great writeup by Anna Debenham](http://24ways.org/2013/get-started-with-github-pages/) which summarizes all the steps I ended up taking to get from Github's instructions to what I wanted, which was to locally preview posts and then push them to be served when I am happy with them.

One thing I find useful that's not mentioned in the writeup above, but is in [the Jekyll docs](http://jekyllrb.com/docs/drafts/), is how to work with *drafts*, posts that live in a `_drafts/` directory, and can be saved, committed and pushed (possibly for an editing pass or collaborative writing) without being posted to the site. In order to preview draft posts, add `--drafts` to your `jekyll serve --watch` command.

# Using Org-mode and org-export-md

So, now you have a repo set up where you can work with Markdown (or Textile or HTML files) and serve up static HTML. The next step is to get from org-mode outlines to the Markdown output that Jekyll needs to do its work. Org-mode has a Markdown exporter available, in my setup I had to add to my `emacs.d/init.el` to put it in the export menu by default:

`;; Add markdown export mode to org defaults`

`(setq org-export-backends (cons 'md' 'org-export-backends))`

When working with your files in org-mode, keep in mind the following points:

-   Adding `#+OPTIONS: toc:nil` to your `.org` files will suppress the creation of an HTML TOC in your Markdown files, which you want because Jekyll won't know what to do with HTML
-   Jekyll requires a YAML front-matter block at the top of each post file. In your `.org` file, this goes in a `#+BEGIN_HTML ... #+END_HTML` block at the top of the file

Your workflow will now look something like this:

-   Edit org-mode files in `org/*.org`
-   Export Markdown: `C-c C-e`, `m`, `M` to get a buffer, then save in `_drafts`
-   Once you're happy with a draft, move it from `_drafts/*.md` to `_posts/*.md` When you push your local changes to your Github Pages repository, anything in `_posts/` will be processed into a real, live HTML blog post.

# Running Jekyll locally

You don't have to push things up to Github to see how Jekyll will render them. You can run it locally and let it process your Markdown locally. Jekyll is a Ruby project, so you will be installing some gems and running with either `jekyll` or `bundle exec jekyll` depending on how comfortable you are having Ruby stuff installed wherever. I prefer to set up a "local" gemset just for my blogging work; it is easy to do with e.g. `rvm use default@jekyll`. Either way you set things up, in your Github Pages repo, you will need a `Gemfile` and a `_config.yml` Here are what mine look like:

## Gemfile

```ruby
source 'https://rubygems.org'
gem 'github-pages'
gem 'therubyracer'
```

## \_config.yml

```yaml
exclude: [org/, Gemfile, Gemfile.lock]
paginate: 5
gems: [jekyll-paginate]
markdown: kramdown
kramdown:
  input: GFM
  smart_quotes: ["apos", "apos", "quot", "quot"]
```

Once you have those set up, you should be able to run `bundle install` followed by `bundle exec jekyll serve --watch --drafts` in a terminal to see how Github Pages will render your blog site.
