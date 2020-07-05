---
layout: post
title: Look around assertions in Perl regular expressions
date: '2014-03-31'
comments: true
categories:
  - perl

---

When Perl's regex engine evaluates a string, it moves from left to right, one
letter at a time checking the match at each position.  That position is called
the *current match position*.

Look around assertions allow you to match a specific pattern before or after
the current match position without moving the match position.  

# Look ahead assertions

Look ahead assertions match the text after the current match position
(without moving the match position).  They look like `(?=pattern)`.

    my $job = "space cowboy";
    $job =~ /space (?=cow)/;    # matches
    $job =~ /space (?=cow)cow/; # also matches

# Look behind assertions

Look behind assertions match the text before the current match position
(without moving the match position).  They look like `(?<=pattern)`.

    my $job = "space cowboy";
    $job =~ /(?<=space) cowboy/;      # matches
    $job =~ /space(?<=space) cowboy/; # also matches

# Positive and negative look ahead assertions

*Positive* look ahead assertions are look ahead assertions which match when their
subpattern matches. They look like `(?=pattern)`.

    my $job = "space cowboy";
    $job =~ /space (?=cowboy)/;   # matches

*Negative* look ahead assertions are look ahead assertions which match when their
subpattern fails. They look like `(?!pattern)`.

    my $job = "space cowboy";
    $job =~ /space (?!mooseboy)/;   # matches

# Positive and negative look behind assertions

Positive look behind assertions are look behind assertions which match when their
subpattern matches. They look like `(?<=pattern)`.

    my $job = "space cowboy";
    $job =~ /(?<=space) cowboy/;   # matches

Negative look behind assertions are look behind assertions which match when
their subpattern fails. They look like `(?<!pattern)`.

    my $job = "space cowboy";
    $job =~ /(?<!earth) cowboy/;   # matches

For more details see `perldoc perlre`.  I also recommend the DuckDuckGo
[regex cheat sheet](https://duckduckgo.com/?q=regex+cheat+sheet).
