---
layout: post
title: Devel::Dwarn helps me type less
date: '2012-09-05'
comments: true
tags: perl

---

2 bajillion times a day I want to print a hashref and see whats inside.
But every time I want to do that I have to type:

    use Data::Dumper::Concise;
    print Dumper $hashref;

First of all, thats too much typing.  And second of all I keep forgetting to
delete my print statements when I check in.

Happily I recently discovered
[Devel::Dwarn](https://metacpan.org/module/Devel::Dwarn) on CPAN.  It is
(basically) an alias to
[Data::Dumper::Concise](https://metacpan.org/module/Data::Dumper::Concise).


The name is shorter which means its slightly less typing so I am already
winning.  But thats not the best part.  Lets see how to use it.

On the command line I do this:

    perl -MDevel::Dwarn codeIWantToDebug.pl

And then in my code I do this:

    print ::Dwarn $hashref;

There are 2 levels of good here:

  1. Less typing.
  2. When I run my code without 'perl -M' I get compile time errors in all
     the places I forgot to remove `print ::Dwarn $hashref`.

