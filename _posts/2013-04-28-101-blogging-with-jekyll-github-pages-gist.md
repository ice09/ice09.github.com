---
layout: post
title: "101 Blogging with Jekyll, GitHub Pages, Gist"
description: ""
category: 
tags: ()
---
{% include JB/setup %}

The following instructions let you install Jekyll on Windows.
Jekyll facilitates really easy blogging without any hosting capabilities, by using a free Github account and Github pages.

# Setup

* Install Ruby: [http://www.ruby-lang.org/de/downloads/](http://www.ruby-lang.org/de/downloads/) (eg Ruby 2.0.0 RubyInstaller \(64 Bit\))
* Install Jekyll: in a command shell, execute `gem install jekyll`
* Most probably, the following exception will appear. Otherwise you can skip to *Install Jekyll Bootstrap*

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
* Start `msys.bat` in the installation directory, then repeat command `gem install jekyll`

# Install Jekyll Bootstrap

* To start with a template blog, Jekyll Bootstrap is really easy to install and [http://jekyllbootstrap.com/](http://jekyllbootstrap.com/)
* Start the local jekyll server: `jekyll --server`
* Install a theme: `rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"`

# Create Github account and Github pages

* Go to [http://www.github.com](http://www.github.com) and create an account
* Create a new repository to host yout blog

<img src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog1_createRepo.jpg" />
<img border="1" src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog2_copyUrl.jpg" />

* Dummy Tip: Install TortioseGit, especially if you are used to TortoiseSVN: [https://code.google.com/p/tortoisegit/wiki/Download](https://code.google.com/p/tortoisegit/wiki/Download)

# Deploying your local changes

* After having tested your blog locally with `jekyll --server` you will finally want to deploy your posts, do this with the following instructions

<pre>
git add .
git commit -m "installed new theme, added new posts"
git push -u origin master
</pre>

* You can then navigate to your blog (it might take some minutes for your changes to take effect)

# Add Post manually in GitHub

* If you want to post from a location without ssh-access, you can even add a new post in the browser (be careful, changes will be online immediately!)

<img border="1" src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog3_createManually.jpg" />

* Remember to merge the changes to your local blog clone with

<pre>
git pull --rebase
</pre>

* after manual changes in your repo