---
layout: post
title: Adding Posts using an Android Phone
categories: jekyll android
---
I thought it would be useful, if i could create a post from my Android Phone. 
A quick search revealed [MrHyde](https://github.com/FauDroids/MrHyde). 

This is an APP that connects to the github API.
It clones the selected repository, and one can edit posts, or create new ones.
It has also a preview Function, which works by sending the changes to an external web server. This builds the pages and returns a preview URL to the APP.

# Problems
This sounds all very nice but there are some problems

* There are unfixed security concerns; e.g. the API key is part oft preview URL. this one is unfixed for over 2 years.
* Currently the preview does not work at all. Maybe the web server used for preview is down. 
* editing Text on a Smartphone does not work well. I just started to edit this post on my smartphone and finished it on my computer. Most annoying is the autocorrection of the smartphone. Of course this has nothing to do with the App.
* there is no markdown support in the app.
* there is no support for images in the app - this would be very nice to take photos using the smartphone and put them into the post from the smart phone.

# Improvements
After all Mr.Hyde is a very clever idea. It has developed quite far and with some work it could be a real killer app. 

* Taking Photos and embedding them would bring the App much forward.

* I also believe that the dependency to an external preview server should be removed. This would require a ruby/jekyll stack on the phone. This is possible, [one way to do it, is using termux](https://github.com/hastebrot/hastebrot.github.io/blob/master/_drafts/lets-setup-jekyll-on-termux.md). Maybe it is possible to package a specialized termux variant which contains only what is necessary for a jekyll build. In principle one could try to run the server part of mr.hyde inside termux. (Ther server is made in python).
