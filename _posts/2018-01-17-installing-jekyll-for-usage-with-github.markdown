---
layout: post
title:  "What i learned during getting started with a webpage on github"
date:   2018-01-17 10:30:55 +0100
categories: jekyll chocolatey
---
## What didn't Work
I followed the recommendation to use jekyll to create web content on github. I had already read about jekyll some time ago in a computer magazine and decided to give it a try. There is a Jekyll Quickstart Guide]( https://jekyllrb.com/docs/quickstart/)

Jekyll depends on ruby - but this should not stop me using it. I havn't written code in ruby yet, i do my scripting mostly in python or as shell scripts. If there are ruby-ish things to learn when using jekyll - why not.

# MSYS2 - fail
The first attempt was to use [MSYS2](http://www.msys2.org/), which like very much and use for my [cross toolchain project](https://github.com/haarer/toolchain68k). The jekyll installation in MSYS2 did'nt work well.

The installation of ruby was no problem but it turned out that the jekyll gem didnt build:

    In file included from C:/tools/msys64/usr/include/ruby-2.3.0/ruby/ruby.h:36:0,
                 from C:/tools/msys64/usr/include/ruby-2.3.0/ruby.h:33,
                 from ruby_http_parser.c:1:
    C:/tools/msys64/usr/include/ruby-2.3.0/ruby/defines.h:61:25: fatal error: sys/select.h: No such file or directory
    # include <sys/select.h>

The build process seems to confuse the MSYS platform with a Cygwin-ish environment - a similar error is reported [here](https://github.com/flori/json/issues/324) without a quick solution.

So i decided to install jekyll using chocolatey. 
# Using Chocolatey - nearly there

I've never used chocolatey but i like the idea to have one interface to the different installation sources for packages. 

Jens Willmer has written how to do it on his [blog](https://jwillmer.de/blog/tutorial/how-to-install-jekyll-and-pages-gem-on-windows-10-x46) but it turned out that there is a dependency problem between ffi (required by jekyll )  and the current ruby version 2.5.
the ffi gem wants a ruby version below 2.5

This can be fixed by setting the ruby version , e.g. `choco install ruby --version 2.4.3.1`

When reading the recommendations on the [jekyll page] (https://jekyllrb.com/docs/windows/#installation) again - they also specify the version, but dont tell why :)

in the further process of installation, they also mention a lenthy command line to install nokogiri - it seems to be that it is no longer necessary.

I also stumbled over the problem that the command 'jekyll new' generated a Gemfile with a  dependency to a jekyll version that does not work with github-pages.

After all, most of the previous steps were unneccesary. A much simpler installation is possible:

## What did work for me
The following steps worked well for me

* In an admin powershell `choco install ruby --version 2.4.3.1`

* In a user powershell go to the github root dir and do `gem install bundle`

* Create an initial Gemfile in the <username>github.io repo root directory 
{% highlight gemfile %}
    source "https://rubygems.org"
    gem "github-pages", group: :jekyll_plugins
    gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]
{% endhighlight %}
* In the powershell do a `bundle update`. This pulls in the proper jekyll version.

* Create the initial page by `jekyll new . -f`.  This overwrites the gemfile, so you need to remove the line requiring the jekyll version in the newly generated file. This is how the gem file should look like.
{% highlight gemfile %}
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]
# add this to autoupdate on changes 
gem 'wdm', '>= 0.1.0' if Gem.win_platform?
{% endhighlight %}
* Serve the page `bundle exec jekyll serve`
* Now you can edit your pages, configure the site in the _config.yml file and so on

As it turns out, there is also a [useful guide on github](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll)