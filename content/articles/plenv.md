---
layout: post
title: Anyenv + Plenv + Carton
date: '2013-11-25'
comments: true
categories:
  - [perl, devops]

---

Recently I started breaking out of my Perl-only isolation bubble and dabbling
with other languages. I was surprised how easy and comfortable other languages
are. I think its because these days good ideas spread from one language to the
next incredibly fast. Even in a new language when I reach for a tool I can
usually find some Perl technology analogue.

For example, I've been a perlbrew person for a long time and it has served me
well.  Ruby has its own version of perlbrew called rbenv which is pretty great.
But actually it seems every language has a clone of rbenv.

 * Python has pyenv.  
 * PHP has phpenv.  
 * Node has ndenv.  
 * Java has jenv.  
 * Perl has plenv.  

So I thought, what the heck -- lets try that out.  The nice thing
is all the envs have pretty much the same command line options and the same
approach to managing dependencies.  I thought it might be calming and soothing
for my brain to have one way of doing things.   

I found [Anyenv](https://github.com/riywo/anyenv) just a second ago as I was
writing this.  It claims it will manage all my envs. Looks like brilliant
stuff. Fortune favors the bold. (I eat danger for breakfast.)  I'll try it.

    git clone https://github.com/riywo/anyenv ~/.anyenv
    echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.my_profile
    echo 'eval "$(anyenv init -)"'               >> ~/.my_profile
    exec bash -l
    anyenv install rbenv    # ruby
    anyenv install plenv    # perl
    anyenv install pyenv    # python
    anyenv install phpenv   # php
    anyenv install ndenv    # nodejs
    anyenv install denv     # dunno
    anyenv install jenv     # java
    exec bash -l            # <-- useful trick btw
    anyenv versions

Ok now I have all the envs.  But I want to actually do some work with plenv.
Lets see if I can do that.

    plenv install --list  # list all the potential perl versions you can use
    plenv install 5.19.6  # install perl v5.19.6
    plenv rehash          # reload the shell environment with the new perl
    plenv global 5.19.6   # use v5.19.6 everywhere by default
    plenv local  5.19.6   # use v5.19.6 in this directory for this project
    plenv install-cpanm   # install cpanm for this version of perl
    plenv rehash          #
    plenv which cpanm     # see where cpanm is installed. should be ~/.anyenv
    
Now I will install the dependencies for my project.  I will manage them with
[Carton](https://metacpan.org/pod/Carton).

    cpanm Carton          # install carton for this version of perl
    plenv list-modules

At this point I need to create a `cpanfile`.  There are all kinds of cool
things you can do in this file, but with your permision I will begin with baby
steps.  Here is mine cpanfile for now:

    requires "Catalyst";
    requires "Plack";
    requires "DBD::SQLite";
       
And then I run carton to install these modules locally into local/lib/perl5.

    carton               # install all the dependencies from the cpanfile
    ls local/lib/perl5/  # see all the new modules installed here
    plenv list-modules   # see mountains of installed stuff
    cd /tmp
    plenv list-modules   # see nothing installed except Carton
    
Notice that a carton.snapshot file was created.  If I look inside, I can see a
list of all my project dependencies, and all their dependencies, etc all the
way down to the first turtle -- AND there are version numbers for everything. 

    # carton snapshot format: version 1.0
    DISTRIBUTIONS
    Apache-LogFormat-Compiler-0.13
      pathname: K/KA/KAZEBURO/Apache-LogFormat-Compiler-0.13.tar.gz
        provides:
          Apache::LogFormat::Compiler 0.13
        requirements:
          CPAN::Meta 0
          CPAN::Meta::Prereqs 0
          Module::Build 0.38
    ...blah biddee blah etc...

I can add `cpanfile` and `cpanfile.snapshot` to my git repo.  Now when I deploy
or share the code, the user at the destination can run Carton and they will end
up with the exact dependencies with the exact same version numbers I had. This
way I can be sure my code will run as successfully for them as it did for me.

    
