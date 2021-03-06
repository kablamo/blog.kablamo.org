---
layout: post
title: Reading code - plackup Architecture
date: '2014-04-09'
comments: true
categories:
  - perl

---

  * [Part 1 - An Overview](/2014/04/08/read-plack-1)
  * [**Part 2 - plackup Architecture**](/2014/04/09/read-plack-2)
  * [Part 3 - PSGI Application Architecture](/2014/04/11/read-plack-3)
  * [Part 4 - Plack::Builder](/2014/04/12/read-plack-4)

# Plack::Runner and plackup

`plackup` starts a PSGI server which executes a PSGI application.  However
the script itself is just a very small wrapper around Plack::Runner which does
all the heavy lifting.  Plack::Runner

  1. parses the command line options.
  2. instantiates the chosen loader class (which is in the Plack::Loader namespace).
  3. instatiates the chosen server library (which is in the Plack::Handler namespace).
  4. starts the PSGI server and passes it a PSGI application

# Plack::Loader

Loaders are responsible for instantiating and running the PSGI server.  Here are
the more interesting capabilities a `$loader` object has:

  * `$loader->guess()` guesses which server library should be loaded by looking at command line opts, $ENV, and %INC.
  * `$loader->load()` instantiates the server library and returns the object.
  * `$loader->run()` starts the server.

The Plack::Loader namespace contains 3 kinds of loaders:

  * Plack::Loader::Delayed - delays compilation of the web app until the first request occurs
  * Plack::Loader::Restarter - reloads the server if any files are changed
  * Plack::Loader::Shotgun - foreach request, forks a child which compiles the web app and runs it

I can choose which loader I want using `plack --loader`

# Plack::Handler

The [PSGI spec](https://metacpan.org/pod/distribution/PSGI/PSGI.pod)
tells me that PSGI defines the interface between an application and a server.
Because the PSGI spec is (intentionally) very minimal, there is a good deal of
wiggle room to interpret how an application and a server might want to play
together.

A library in the Plack::Handler namespace is the place where the application
meets the server.  This layer contains all the wiggling.

Lets say I wrote a new server called AngryBrontosaurus and I want to be able to
use it with `plackup --server AngryBrontosaurus`.  I could implement a small
class like this:

    package Plack::Handler::AngryBrontosaurus
    use strict;
    use AngryBrontosaurus;

    sub new {
        my $class = shift;
        bless { @_ }, $class;
    }

    sub run {
        my ($self, $app) = @_; 
        AngryBrontosaurus->new->run($app, $self);
    }

Then, to make sure AngryBrontosaurus and Plack::Handler::AngryBrontosaurus
correctly implement the PSGI spec, I should also test my code with
Plack::Test::Suite.

    use Test::More;
    use Plack::Test::Suite;
    Plack::Test::Suite->run_server_tests('AngryBrontosaurus');
    done_testing;

Notice that while the Plack::Handler namespace contains classes for several
PSGI servers like Plack::Handler::Starman or Plack::Handler::Twiggy, it also
includes some classes like Plack::Handler::Apache2 and Plack::Handler::FCGI.
Clearly Apache2 was not written with PSGI compliance in mind, but there is glue
in the Plack::Handler::Apache2 layer to enable it to speak with PSGI compliant
applications.


# Sequence diagram

This diagram describes how Plack::Runner, Plack::Handler, and Plack::Loader
interact.

![x](/images/for-posts/2014-04-01-plack.png)

