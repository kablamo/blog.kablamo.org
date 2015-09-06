---
layout: post
title: Dist::Zilla::PluginBundle::DAGOLDEN is pretty awesome
date: '2013-02-20'
comments: true
categories: perl

---

I've been using [Dist::Zilla](https://metacpan.org/module/Dist::Zilla) for a
couple years.  It's a powerful way to automate CPAN releases.  

But learning how to use it was not as easy as I hoped.

I remember when `Dist::Zilla` first debuted.  It was very exciting.  But I think
I may have drank too much of the cool aid becuase my expectations were very
high when I finally sat down to learn it.  I expected my experience to be
composed entirely of rainbows and puppy dog tails.

Writing your own dist.ini or PluginBundle is hard
-------------------------------------------------

Instead I found that creating a dist.ini or PluginBundle is fairly hard.  There
are a huge number of plugins and it's difficult for a newcomer to know which
are old, which are new, and how they work together.  If you look, for example,
at `Dist::Zilla::PluginBundle::DAGOLDEN` it uses 23 different plugins and the
*synopsis* is 132 lines long.

In retrospect, it was not reasonable to expect I could build something
comparable after a few minutes of perusing the docs.  It's more complex than
that.  So if you are looking to quickly add `Dist::Zilla` to your toolchain, you
need to use a PluginBundle and not write your own.

How to quickly add Dist::Zilla to your toolchain
-----------------------------------------------

One way is to just use `Dist::Zilla::PluginBundle::Basic`.  But this was not like
the promised land I had been dreaming of.  I wanted more.  So I kept looking.

Happily, there is a PluginBundle which I think works well as a reusable
component suitable for public consumption thats also very configurable.  I
doubt it's well known because the name sounds very personal.  That module is,
of course,
[Dist::Zilla::PluginBundle::DAGOLDEN](https://metacpan.org/module/Dist::Zilla::PluginBundle::DAGOLDEN).

I think the workflow it uses will work for many people.  Even if it
doesn't, reading the code is a great way to learn how to write your own
PluginBundle.  And because it's so comprehensive it's like having a up-to-date
map of the state of the art in `Dist::Zilla` plugins and how they work
together.  

Here's what my dist.ini looks like:

    name    = App-Git-Ribbon
    author  = Eric Johnson <cpan at iijo dot nospamthanks dot org>
    license = Perl_5
    copyright_holder = Eric Johnson
    main_module = lib/App/Git/Ribbon.pm

    [@DAGOLDEN]
    no_spellcheck = 1
    AutoMetaResources.bugtracker.rt = 0
    AutoMetaResources.repository.github = user:kablamo
    AutoMetaResources.bugtracker.github = user:kablamo
    weaver_config = @FLORA

