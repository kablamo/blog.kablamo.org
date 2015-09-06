---
layout: post
title: I made a Readline cheat sheet
date: '2014-01-01'
comments: true
tags: perl

---

Recently Ovid pointed out [large projects are much more likely to fail](http://blogs.perl.org/users/ovid/2014/01/ditching-a-language.html).
I have a few large goals I'd like to accomplish.  For example I want to improve
my front end design skills.  Rather than trying to tackle this problem all
at once, I made up a small project for myself.

I created a [Readline cheat sheet](http://readline.kablamo.org/emacs.html) and
I was able to complete this project in about a day.  Here are some
of the things I learned:

 * **Bootstrap** - I always worry libraries and frameworks like
[Bootstrap](http://getbootstrap.com) are overkill and bloat since I only need
a tiny portion of their features.  But its undeniable that I was able to quickly
build a responsive mobile friendly website without needing to worry about the
technical details.

 * **Readline commands** - If I had to pick one keyboad shortcut to recommend it
would be <code>Ctrl-r</code> which allows you to search backwards through your
history.  I also like the incremental undo command: <code>Ctrl-\_</code>.

 * **Text::Xslate** - People keep mentioning
[Text::Xslate](https://metacpan.org/pod/Text::Xslate) so I wanted to give it a try.
The docs say its full featured and very fast.  I liked that HTML metacharacters
are escaped by default to avoid cross site scripting attacks.  Also it supports
Template Toolkit syntax.  I didn't find any new killer features, but it was a
pleasure to work with.

 * **Some new [CSS tricks](http://helabs.com.br/blog/2014/01/21/prevent-common-problems-when-writing-css-from-scratch/)** -
The best trick I learned is how to keep my footer at the bottom of the page
even when it has only a few lines of content.

I was able to practice design, layout, color, and font selection.  And who
knows -- perhaps this project will also drive a tiny bit of traffic to my
github profile and increase my
[luck surface area](http://www.codusoperandi.com/posts/increasing-your-luck-surface-area).

The momentum feels good.  I need to remember to keep my projects small more often.

![x](/images/for-posts/2014-02-02.png)

