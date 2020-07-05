---
layout: post
title: CPAN::Diff - Compare local Perl modules to your darkpan or cpan
date: '2016-10-07'
comments: true
categories:
  - [perl]

---

At work we have a Pinto powered darkpan.  But developers install stuff from
cpan on their development servers to try things out and occasionally these
experiments don't get cleaned up and cause problems.  It would be nice nice to
know what modules are installed on a machine and how that compares to whats on
our darkpan.  Specifically I want to know which modules are:

 - Older than those on the darkpan
 - Newer than those on the darkpan
 - Installed on the server but are not in the darkpan

I solved this by stealing a lot of code from
[cpan-outdated](https://metacpan.org/pod/cpan-outdated) and writing
[CPAN::Diff](https://metacpan.org/pod/CPAN::Diff):

    # Usage
    $ cpan-diff --help
    
    # Find local modules which are older than whats available in the CPAN
    $ cpan-diff older
    Acme::LookOfDisapproval
    Acme::What
    
    $ cpan-diff older --verbose
    Acme::LookOfDisapproval        0.005   0.006 ETHER/Acme-LookOfDisapproval-0.006
    Acme::What                     0.004   0.005 T/TO/TOBYINK/Acme-What-0.005.tar.gz
    
    # Find local modules which are older than the ones in your company darkpan.
    $ cpan-diff older --verbose --mirror https://darkpan.yourcompany.com
    Acme::LookOfDisapproval        0.005   0.006 ETHER/Acme-LookOfDisapproval-0.006
    Acme::What                     0.004   0.005 T/TO/TOBYINK/Acme-What-0.005.tar.gz
    
    # Find local modules which are newer than the ones in your darkpan.  
    $ cpan-diff newer --mirror https://darkpan.yourcompany.com
    
    # Find local modules which are 'extra' -- ie don't exist in your darkpan.
    $ cpan-diff extra --mirror https://darkpan.yourcompany.com



