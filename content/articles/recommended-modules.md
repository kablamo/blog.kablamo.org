---
layout: post
title: What's the best Perl module for X
date: '2018-03-10'
comments: true
categories:
  - perl

---

Its hard for beginners and experts alike in any language to keep up with the
flood of new libraries being written all the time.  MetaCPAN has thousands of
modules. Its hard to know which module is the best one. For example there are
at least 7 modules for parsing JSON in Perl. New modules appear all the time
which make the old best solution obsolete. 

# How to choose a module

There are a couple good sources of advice:

- [mstpan](http://blog.kablamo.org/2015/09/08/mstpan/) is a great
  set of blog posts from 2015 written by
[mst](https://shadow.cat/blog/matt-s-trout/).  
- [Task::Kensho](https://metacpan.org/pod/Task::Kensho) is a list of approved
  modules maintained by The Enlighted Perl Organization. 
- [Neil Bowers](http://neilb.org/) wrote some [great module reviews](http://neilb.org/reviews/).
- [DiscuragedModules](https://metacpan.org/pod/Perl::Critic::Policy::Freenode::DiscouragedModules)
  is a perlcritic policy which lists modules you probably shouldn't use.

# Some problems

These are great sources of advice that insiders are familiar with.  There are few
issues though.

- This advice is hard to find for new people -- or
  even for people who are busy solving problems instead of navel gazing about
  their language of choice (like I do).
- There isn't always a sustained effort to keep these current.
- Some sources don't discuss alternatives or provide reasons for the
  recommendations.  A discussion helps developers make up their own mind about
  whether the advice fits their situation or is outdated or is just silly.
- Some topics aren't covered.

# Recomended Modules

I'm hoping I can solve some of these issues.  Its still new and experimental,
but I started writing a new chapter for my book Minimum Viable Perl called
"Recommended Modules".  

The first article is out and its about choosing [the best module for handling
exceptions](http://mvp.kablamo.org/cpan/exceptions/).  I reviewed 6 different
ways of handling exceptions and benchmarked them.  Its the 1st of 3
articles on handling exceptions.

The goal is to make the "Recommended Modules" chapter everyone's destination
for quickly discovering what the best Perl module is for solving a given
problem.  

Its on github -- like the rest of the book.  So feedback is welcome via [github
issues](https://github.com/kablamo/mvp.kablamo.org/issues).

<img style="font-size: 20rem" class="emojidex-emoji" src="https://cdn.emojidex.com/emoji/seal/thumbsup(ye).png" emoji-code="thumbsup(ye)" emoji-moji="👍🏽" alt="thumbsup(ye)" />
