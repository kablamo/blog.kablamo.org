---
layout: post
title: Configure tmux to give you status updates about anything
date: '2013-11-26'
comments: true
categories: tools

---

I use and love OSX growl and Ubuntu notify style desktop notifications.  They
are gorgeous.  But they are also distracting and after a few vibrant and
whimsical but fleeting seconds they are gone forever and you have no way to get
them back.

A more useful (and less glorious) way to receive notifications is in your
[tmux](http://tmux.sourceforge.net/) status bar.  tmux is the brilliant
successor to the venerable screen which hasn't been actively developed for
quite a while.

Here is how I configure notifications in my .tmux.conf file:

    set -g status-interval 15    
    set -g status-right !exec my_shell_script

This tells tmux to run `my_shell_script` every 15 seconds.  It displays the
first line of output from the shell script.  Now I can get unobtrusive status
updates which don't go away. And if I ate a good breakfast, feel rested, and
have the wind at my back I can write some code to log my notifications to a
file so that I don't miss any.

Here are some ideas that might be useful which I might someday do maybe perhaps
possibly:

 * Status updates when someone says your name on irc
 * Status updates when jenkins tests fail
 * Status updates when people push code live
 * Status updates when people merge branches

Any other ideas?  

Also don't miss the tmux [powerline](https://github.com/Lokaltog/powerline)
status bar.
