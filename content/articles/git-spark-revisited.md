---
layout: post
title: git-spark revisited
date: '2013-01-27'
comments: true
categories:
  - perl

---

A [few days ago](http://blog.kablamo.org/git-spark-plots-your-commit-history/)
I wrote about my [git-spark](https://gist.github.com/4598480) Perl script.  It
counts how many commits a user has in a git project and makes a little graph
and displays it on the command line.

However I also said it wasn't very useful becuase you can't compare one graph
with another because the scale changes when different graphs have different min
and max values.  For example these two data series produce identical graphs
despite have very different data.

    ⚡ spark 1 2 3 4 5
    ▁▂▄▆█
    ⚡ spark 10 20 30 40 50
    ▁▂▄▆█

So I put on my thinking cap and came up with the following solution: 

    ⚡ spark 50 1 1 2 3 4 5
    █▁▁▁▁▁▁
    ⚡ spark 50 1 10 20 30 40 50
    █▁▂▃▅▆█

I just need to prepend a max and a min to the data to get consistent scaling
and now I can compare graphs.

For git-spark, I now assume the min is zero and you can pass in the max using
the --scale option.  (Note that I chopped off the max/min characters from
the spark output as they are distracting.)

I also decided to print out the number of commits which helps with the
scaling issue.  And while I was in there I got it to calculate the total,
average, and maximum number of commits for that duration.

Here is an example.  It doesn't really need the --scale option because the data
is so close anyway, but it shows how to use it:

    ⚡ git spark --days 14 --scale 23 Stegosaurus
    Commits by Stegosaurus over the last 14 days
    total: 95   avg: 7   max: 23
    10 15 6 23 5 0 0 1 15 0 17 3 0 0
    ▄▅▂█▂▁▁▁▅▁▆▁▁▁
    ⚡ git spark --days 14 --scale 23 Triceratops
    Commits by Triceratops over the last 14 days
    total: 90   avg: 7   max: 22
    1 12 3 11 3 0 0 6 16 3 13 22 0 0
    ▁▄▁▄▁▁▁▂▅▁▄▇▁▁
    
Of course you still need to consider the quality of commits and not just how
many there are.  
