---
layout: post
title: git-vspark plots your commits vertically with Term::Vspark
date: '2013-03-17'
comments: true
categories: perl

---


I added a **git-vspark** script to my App::Git::Spark CPAN module.  It does the
same thing as [git-spark](https://github.com/kablamo/git-spark) but instead of
normal horizontal sparklines, it uses "vertical" sparklines.  Here's what that
looks like:

    $ git vspark --months 8 batman
    Commits by batman over the last 8 months
    total: 233   avg: 29   max: 69
     12 ██▋
     18 ████
     69 ███████████████▏
     59 ████████████▉
     16 ███▌
     28 ██████▏
     12 ██▋
     19 ████▎

This effect is achieved using
[Term::Vspark](https://metacpan.org/module/Term::Vspark).  Its companion module
[Term::Spark](https://metacpan.org/module/Term::Spark) is a small pure Perl
replacement for Zach Holman's original [spark](https://github.com/holman/spark)
implementation and it now powers my git-spark script.  

These libraries were fun little projects developed over the past few weeks
mainly by [Gil Gonçalves](https://github.com/LuRsT) with a few pull requests
from myself.  Having them available on CPAN means you can easily use sparklines
in your own Perl code.  


