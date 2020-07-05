---
layout: post
title: "Bash tricks: set -euxo pipefail"
date: '2015-11-08'
comments: true
categories:
  - [bash, rex]

---

`set -eux` is a super useful bash trick I've been using in Chef and
[Rex](https://metacpan.org/pod/Rex) tasks.  I'm going to break it down and
explain it one option at a time:

# set -e
This

    cmd1 && cmd2 && cmd3

is equivalent to this

    set -e
    cmd1
    cmd2
    cmd3

# set -u
The shell prints a message to stderr when it tries to expand a variable that is
not set.  Also it immediately exits. An interactive shell will not exit.  I
think this is similar to Perl's 
[use strictures](https://metacpan.org/pod/strictures) which is something Moo
enables.

# set -x
The shell prints each command in a script to stderr before running it.  I think
this would be particularly useful in Rex.  And Chef.

    set -x
    echo hey
    echo woah

output:

    + echo hey
    hey
    + echo woah
    woah

# set -o pipefail

Pipelines fail on the first command which fails instead of dying later on
down the pipeline.  This is especially good when cmd3 is a command that always
succeeds (like echo):

    cmd1 | cmd2 | cmd3

# See also
 - [The most useful bash trick of the year](http://www.peterbe.com/plog/set-ex)
 - [explainshell: set -euxo pipefail](http://explainshell.com/explain?cmd=set+-euxo pipefail)
