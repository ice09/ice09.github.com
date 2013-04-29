---
layout: post
title: "101 Blogging with Jekyll, GitHub Pages, Gist"
description: ""
category: 
tags: ()
---
{% include JB/setup %}

# Setup

* Install Ruby: [http://www.ruby-lang.org/de/downloads/](http://www.ruby-lang.org/de/downloads/) (eg Ruby 2.0.0 RubyInstaller \(64 Bit\))
* Install Jekyll: `gem install jekyll`

<pre>
C:\Windows\system32>gem install jekyll
Fetching: liquid-2.5.0.gem (100%)
Successfully installed liquid-2.5.0
Fetching: fast-stemmer-1.0.2.gem (100%)
ERROR:  Error installing jekyll:
        The 'fast-stemmer' native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions
at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
</pre>

* Choose the corresponding DevKit at [http://rubyinstaller.org/downloads](http://rubyinstaller.org/downloads) (eg. mingw64-64-4.7.2)
* Start `msys.bat`, repeat command `gem install jekyll`
* Dummy Tip: Install TortioseGit, especially if used to TortoiseSVN: [https://code.google.com/p/tortoisegit/wiki/Download](https://code.google.com/p/tortoisegit/wiki/Download)
* [http://jekyllbootstrap.com/](http://jekyllbootstrap.com/)
* Start the local jekyll server: `jekyll --server`
* Install a theme: `rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"`

<pre>
git add .
git commit -m "installed new theme"
git push -u origin master
</pre>

# Add Post manually in GitHub

<img src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog1_createRepo.jpg" />

<img border="1" src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog2_copyUrl.jpg" />

<img border="1" src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog3_createManually.jpg" />