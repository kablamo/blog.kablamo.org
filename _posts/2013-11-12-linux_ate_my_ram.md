---
layout: post
title: My contribution to the Linux ate my RAM problem
date: '2013-11-12'
comments: true
tags: bash

---

I can never remember how to know how much free memory I have.

The Linux kernel claims most of the operating system's memory.  That doesn't
mean the operating system is out of memory.  It means the kernel has claimed it
and is managing it.  The problem is that the Linux kernel defines 'free' memory
differently than any reasonable user.  There are excellent shiny reasons for
that, but as a user I don't really care.

    ~ ⚡ free -m 
                total       used       free     shared    buffers     cached
    Mem:          3822       2066       1755          0        147       1125
    -/+ buffers/cache:        793       3029
    Swap:         3394          0       3394

Experienced users and sysadmins will know from this means my system currently
has 3822 MB of total memory and 3029 MB of free memory.  Errr.  What?  See
[linuxatemyram.com](http://www.linuxatemyram.com) for a more detailed
explanation.  

I think the world deserves something easier to deal with.  So I wrote 'mem'.
Use it like this:

    ~ ⚡ mem
       927 MB used (24%)
      3823 MB total

Its a bash function.  I put the following in my
[.bashrc](https://github.com/kablamo/dotfiles/blob/master/links/.bash/aliases.sh):

    mem() {
        memfree=$( grep '^MemFree:' /proc/meminfo | awk '{ mem=($2)/(1024) ; printf "%0.0f", mem }' )
        buffers=$( grep '^Buffers:' /proc/meminfo | awk '{ mem=($2)/(1024) ; printf "%0.0f", mem }' )
        cached=$(  grep '^Cached:'  /proc/meminfo | awk '{ mem=($2)/(1024) ; printf "%0.0f", mem }' )
        free=$( echo $memfree+$buffers+$cached | bc -l )

        total=$( grep '^MemTotal:' /proc/meminfo | awk '{ mem=($2)/(1024) ; printf "%0.0f", mem }' )
        used=$( echo $total-$free | bc -l )
        pct=$( echo 100*$used/$total | bc -l )

        printf "%5.f MB used (%.0f%%)\n%5.f MB total\n" $used $pct $total
    }

See also 'htop' (sudo apt-get install htop).
