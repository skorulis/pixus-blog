---
layout: post
title:  "First steps"
date:   2015-05-16 12:11:05
categories: setup pixus
---
Pixus is a project that I've been working on for some time while a startup that I've been working for called [lenslife] looks to get funding to continue development. Today my goal is to start letting people know about what I'm doing because if no one knows about it then how can it ever be successful?

I've decided to build a blog using [jekyll] which takes markdown posts and generates HTML documents. This seems like a nice middle ground between having a WYSIWYG blog editor like medium and having to build full HTML pages. Since my background lies in iOS development I feel like an absolute novice setting these things up. Getting this up and running and being able to view the generated site has been really easy. The built site doesn't work when running it locally but it's fine if I run it in a server using `python -m SimpleHTTPServer 8000`

So now that I've got the framework for building a blog I need to find somewhere to host it. Jekyll is hosted on [GitHub Pages] and I've seen it mentioned in a few other places so I decided to try it out first. Github actually uses Jekyll directly so all I need to do is push my blog to a new repository with a `gh-pages` branch. Sadly the site itself didn't work as expected but looked exactly how it did when running from a `file://` version. Luckily [Jekyll have a page with instructions](http://jekyllrb.com/docs/github-pages/) for how to fix this. Adding `baseurl: /pixus-blog` to my config file made the page work correctly. I guess if I used Github's wizard this would have been done for me, but as always I like to do things the hard way.

Next up I wanted to give the blog a nice domain using my recently purchased pixus.io. [Github have a page exlaining this](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/) though many of the concepts are new to me.

[GitHub Pages]: http://pages.github.com/
[jekyll]:      http://jekyllrb.com
[lenslife]:      http://http://lenslife.com.au
