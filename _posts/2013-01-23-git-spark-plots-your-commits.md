---
layout: post
title: git-spark plots your commit history
date: '2013-01-23'
comments: true
categories: perl

---

I recently rediscovered [spark](https://github.com/holman/spark), Zach Holman's
cool little sparklines graphing tool for the command line.  I used a little
Perl to mash it up with 'git log' and came up with
[git-spark](https://gist.github.com/4598480) which works like this:

    ⚡ git spark --hours 8
    Commits by Godzilla over the last 8 hours
    ▃▃▁▆▅▁▁▃█
    ⚡ git spark -d 14 HulkHogan
    Commits by HulkHogan over the last 14 days
    ▇▅▄▁▁▄▅▂█▂▁▁▁▅
    ⚡ git spark -w 52 Tarzan
    Commits by Tarzan over the last 52 weeks
    ▃▁▂▃▃▃▂▁█▆▁▄▄▃▂▂▁▁▂▃▃▄▃▃▂▃▁▁▁▁▁▂▂▃▆▅▂▁▄▃▂▄▄▄▁▂▁▁▂▂▂▃

And heres the usage/help:

    ⚡ git spark -h
    usage: git spark [-dhmowy] [long options...] [AUTHOR]
            -o --hours      commits from the last x hours
            -d --days       commits from the last x days
            -w --weeks      commits from the last x weeks
            -m --months     commits from the last x months
            -y --years      commits from the last x years
            -h --help       show this message

It was fun to build, but afterward I realized its totally useless.  Clearly
'commits' are a problematic metric.  But its much worse than that.  The peaks
on the graph are relative to the lows on the same graph.  So a peak on one
graph has no relation to a peak on another.  That means I can't compare one
sparkline with another.

Back to the drawing board.  I'll have to come up with something else.  

**UPDATE:** I solved this problem in [git-spark revisted](http://blog.kablamo.org/git-spark-revisited/).
