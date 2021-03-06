---
layout: post
title: Reading code - Plack::Builder
date: '2014-04-12'
comments: true
categories:
  - perl

---

  * [Part 1 - An Overview](/2014/04/08/read-plack-1)
  * [Part 2 - plackup Architecture](/2014/04/09/read-plack-2)
  * [Part 3 - PSGI Application Architecture](/2014/04/11/read-plack-3)
  * [**Part 4 - Plack::Builder**](/2014/04/12/read-plack-4)

Plack::Builder provides a domain specific language (DSL) for middleware
developers.  It looks like this:

    use Plack::Builder;

    my $app1 = sub { ... };
    my $app2 = sub { ... };

    builder {
        enable "Deflater";
        enable "Session", store => "File";
        enable "Debug", panels => [ qw(DBITrace Memory Timer) ];

        mount "/narwhale" => $app1;
        mount "/unicorn"  => $app2;
    };

How does it work?  With three artful tricks.

# Artful trick #1

The first artful trick is the `builder` block. 

    sub builder(&) {
        my $block = shift;
        ...
    }

The `&` is a function prototype.  Perl offers some limited compile time checking
for parameters passed to subs.  Here is what `perldoc perlsub` says about `&`:

> An "&" requires an anonymous subroutine, which, if passed as the first
> argument, does not require the "sub" keyword or a subsequent comma.

So if I try to pass `builder()` a scalar or an array or anything thats not an
anonymous subroutine, I will get a compile time error.  But if I pass it an
anonymous subroutine, the compiler will allow things to continue.

# Artful trick #2

The next artful trick is that Plack::Builder implements the DSL keywords as
subs and then exports those subs.

    package Plack::Builder;
    use strict;
    use parent qw( Exporter );
    our @EXPORT = qw( builder enable enable_if mount );
    ...
    sub enable    {...}
    sub enable_if {...}
    sub mount     {...}
    # etc

Actually thats 90% of the whole thing isn't it?  Now its starting to look
obvious.  But lets continue.

# Artful trick #3

There is one more interesting idea here.  Notice that if I use `enable`,
`enable_if`, or `mount` outside of a `builder` block I will get an
error.  This works because the DSL keywords are subs which run code references.
By default those code references refer to code which croaks an error.  But when
`builder` runs, those references are temporarily replaced with real working
code.

Here's some simplified code to illustrate how it works.

    our $_enable = sub { Carp::croak(...) }; # << default code reference

    sub enable { $_enable->(@_) }

    sub builder(&) {
        my $block = shift;
        ...
        local $_enable = sub {...}; # << temporarily assign real working code
        ...
        my $app = $block->();
        ...
    }
    

    



