---
layout: post
title: Reading code - PSGI Application Architecture
date: '2014-04-11'
comments: true
categories:
  - perl

---

  * [Part 1 - An Overview](/2014/04/08/read-plack-1)
  * [Part 2 - plackup Architecture](/2014/04/09/read-plack-2)
  * [**Part 3 - PSGI Application Architecture**](/2014/04/11/read-plack-3)
  * [Part 4 - Plack::Builder](/2014/04/12/read-plack-4)

# PSGI Applications

The [PSGI spec](https://metacpan.org/pod/distribution/PSGI/PSGI.pod) defines
a PSGI application.

> A PSGI application is a Perl code reference. It takes exactly one argument,
> the environment, and returns an array reference containing exactly three
> values.

The three values are a status, headers, and a body.  Here is an example:

    my $app = sub {
        my $env = shift;
        return [
            '200',
            [ 'Content-Type' => 'text/plain' ],
            [ "Hello World" ], # or IO::Handle-like object
        ];
    };

# The PSGI environment hash

The PSGI environment hash is a hashref with many keys.  But mostly it is the
data (headers, body, etc) from an HTTP::Request which has been parsed and put into
a hash for convenient access.

# Middleware

A middleware component takes a PSGI application and runs it, passing in the
PSGI environment hash.  But before it runs the app, it gets to modify the
environment if it wants to.  And after running the app, it can modify the
response if it wants to.


# Plack::Middleware

Middleware is a wrapper around a PSGI app.  More than one middleware can be
wrapped around an app, creating a series of layers like an
[onion](http://blogs.perl.org/users/jakob/2012/09/28/middleware-onion.png/500px-MiddlewareOnion.svg.png).
What makes the middleware onion a somewhat unusual construct is the event
driven / callback nature of it.  Lets look at how its implemented.

All middleware inherits from Plack::Middleware which is an itsy bitsy (teeny
weeny) module.  The middleware onion is created with just 2 short subroutines
(notice the `call()` and `prepare_app()` subs are written by middleware authors):

    sub wrap {
        my($self, $app, @args) = @_;
        if (ref $self) {
            $self->{app} = $app;
        } else {
            $self = $self->new({ app => $app, @args });
        }
        return $self->to_app;
    }

    sub to_app {
        my $self = shift;
        $self->prepare_app;
        return sub { $self->call(@_) };
    }

How do these subs work together?  The middleware onion is sometimes constructed as follows:

    my $app = MyWebApp->new->to_app;
    $app = Plack::Middleware::A->wrap($app);
    $app = Plack::Middleware::B->wrap($app);
    $app = Plack::Middleware::C->wrap($app);

But it might be more clear to write it this way

    my $app0 = MyWebApp->new->to_app;           # $app0->($env) runs the web app
    $app1 = Plack::Middleware::A->wrap($app0);  # $app1->($env) calls P::M::A->call() which calls $app0->($env)
    $app2 = Plack::Middleware::B->wrap($app1);  # $app2->($env) calls P::M::B->call() which calls $app1->($env)
    $app3 = Plack::Middleware::C->wrap($app2);  # $app3->($env) calls P::M::C->call() which calls $app2->($env)
                                                # When the server receives a request it calls $app3->($env)

So when an event occurs -- for example the PSGI server sees a new request -- it
passes the event to the app.  The app is a chain of callbacks which run each
other.  This is clearly an example of event driven programming.


# Plack::Component and Plack::App

Plack::Middleware inherits from Plack::Component.  So the most common use of
Plack::Component is in middleware.

Plack::Component can also be used as a tool for creating PSGI applications.  It
has a light dusting of code, but mostly its an interface which is implemented
by modules in the Plack::App namespace.  For example Plack::App::File is a web
app which serves static files from a root directory, and Plack::App::URLMap is
a web app which maps multiple web apps to multiple urls.

But notice that I am not required to use Plack::Component to create a PSGI
application. A PSGI application is just a code reference.  The PSGI spec does
not say that a PSGI application is a reference to code that inherits from
Plack::Component.

The nice thing about using Plack::Component to build my app is that it
provides a common interface for all PSGI apps.  Whenever I see `$app`, I
can rely on that behavior.  This is clearly important for middleware.  And it
feels good from a design point of view.

But its not required and it adds some complexity.


