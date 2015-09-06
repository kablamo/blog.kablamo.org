---
layout: post
title: Move fast, break nothing (tldr)
date: '2015-06-16'
comments: true
categories: [strategy, tldr]

---

[Move fast & break nothing](http://zachholman.com/talk/move-fast-break-nothing/) 
is a great talk about "code, teams, and process" by Github's Zach Holman.  Its
kind of long though.  Here is my summary

 * Kittens and dogs evolved to be cute so we don't eat them
 * Github deploys code hundreds of times a day
 * Automated tests help devs move fast and break less stuff
 * Parallel code paths as a way to test changs to your production website: 
   * Run new code on 1% of requests, run old code on 99% of requests
   * Switch to the new code once you are confident it works
 * Processes are [checklists](http://www.nytimes.com/2009/12/24/books/24book.html?pagewanted=all&_r=0).  They:
   * Help get the stupid stuff right
   * Remove ambiguity
 * Adding process is expensive. Instead grow processes laterally.  eg Instead of adding an additional step like running tests, make running tests part of the existing `git checkin` process.
 * Automated tests are a lower cost way to add process laterally.
 * Visible ownership is great for open source projects because new people know who to ask.  Github has an OWNERS file in the repo.
 * Ownership encourages mentorship/spreads knowledge because people know who to ask.
 * Github includes ownership/Area Of Responsibility in their production logs.
 * Feedback hurts and can be rude.  Don't let that stop you from seeking it out.

