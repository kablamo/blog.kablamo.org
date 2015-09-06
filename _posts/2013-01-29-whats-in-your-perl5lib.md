---
layout: post
title: Whats in your $PERL5LIB?
date: '2013-01-29'
comments: true
categories: perl

---

Ever wonder whats in your $PERL5LIB?  Here is one way to find out:

    ⚡ echo $PERL5LIB
    .:./lib:/home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/perl5:/home/eri
    c/perl5/perlbrew/perls/perl-5.16.2/lib/perl5/i686-linux:/home/eric/perl
    5/perlbrew/perls/perl-5.16.2/lib/perl5/i686-linux-gnu-thread-multi-64in
    t:/home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/site_perl:/home/eric/
    perl5/perlbrew/perls/perl-5.16.2/lib/5.16.2
 
My human eyeballs are not equipped to parse that.  Unhelpful.  So I put this in
my .bashrc:

    alias perl5lib='perl -E "say join \"\n\", split \":\", \$ENV{PERL5LIB}"'

Here it is in action:

    ⚡ perl5lib
    .
    ./lib
    /home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/perl5
    /home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/perl5/i686-linux
    /home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/perl5/i686-linux-gnu-thread-multi-64int
    /home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/site_perl
    /home/eric/perl5/perlbrew/perls/perl-5.16.2/lib/5.16.2
    
Better.
