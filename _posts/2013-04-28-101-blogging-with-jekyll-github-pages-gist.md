---
layout: post
title: "101 Blogging with Jekyll, GitHub Pages, Gist"
description: "Instructions for enabling blogging with Windows"
category: blogging
tags: (jekyll, ruby, blogging, gh-pages)
---
{% include JB/setup %}

# Finally: free, easy and fun blogging ... using Windows!

[Jekyll](http://jekyllrb.com/) facilitates really easy blogging without any hosting capabilities, by using a free [GitHub](http://www.gihub.com) account in combination with [GitHub Pages](http://pages.github.com/).<br/>
The following instructions let you install [Jekyll](http://jekyllrb.com/) on Windows.

# Setup

* Install [Ruby](http://www.ruby-lang.org/de/downloads/) for Windows (eg. Ruby 2.0.0 RubyInstaller \(64 Bit\))
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

* Choose the corresponding [DevKit](http://rubyinstaller.org/downloads) (eg. mingw64-64-4.7.2)
* Start `msys.bat` in the installation directory, then repeat command `gem install jekyll`

# Create Github account and Github pages

* Go to [GitHub](http://www.github.com) and create an account
* Create a new repository to host your blog

<img src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog1_createRepo.jpg" />

* after the creation of the repository note down the GitHub URL, you will need this for modifying your blog, eg. with Jekyll Bootstrap (next step)

<img src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog2_copyUrl.jpg" />

* Dummy Tip: Install [TortioseGit](https://code.google.com/p/tortoisegit/wiki/Download), especially if you are used to TortoiseSVN.

# Install Jekyll Bootstrap

* To start with a template blog, [Jekyll Bootstrap](http://jekyllbootstrap.com/) is really easy to install and use
* Start the local jekyll server: `jekyll --server`
* Optional: Install a theme: `rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"`

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

<img src="/assets/2013-04-28-101-blogging-with-jekyll-github-pages-gist/img/blog3_createManually.jpg" />

* Remember to merge the changes back to your local blog clone with

<pre>
git pull --rebase
</pre>

after doing direct manual changes in your repo

# Add source code snippets to your posts

* Go to [Gist](https://gist.github.com/) and create one
* Copy the link from *Embed this link*
* Embed the link into the page, *note that you have to fill in a space between opening and closing script tag*

<pre>
&lt;script src="https://gist.github.com/ice09/5497460.js"&gt; &lt;/script&gt;
</pre>

Results in nice, reusable, embedded code snippets like this

<script src="https://gist.github.com/ice09/5497460.js" type="text/javascript"> </script>

<script src="https://gist.github.com/2603939.js?file=gist%2Bexample.m"> </script>
