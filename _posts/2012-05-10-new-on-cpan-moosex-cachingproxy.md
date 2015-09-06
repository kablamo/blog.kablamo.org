---
layout: post
title: New on CPAN - MooseX::CachingProxy
date: '2012-05-10'
comments: true
categories: perl

---

Last week I released [MooseX::CachingProxy](https://metacpan.org/module/MooseX::CachingProxy) to CPAN.

Its a small module that intercepts requests from your LWP based application.
Those requests are relayed on to the intended server unless they already exist
in the cache.

To toggle on and off the caching proxy, MooseX::CachingProxy provides the
attribute 'caching\_proxy'.  Here is a quick demo:

    package MyApp;
    use Moose;
    use WWW::Mechanize; # or any LWP based library
    with 'MooseX::CachingProxy';

    sub url { 'http://example.com' } # required by MooseX::CachingProxy

    sub download { 
        my $self = shift;
        $self->start_caching_proxy;
        return WWW::Mechanize->new()->get($self->url . '/foo'); 
    }

Under the covers, its a tiny Plack application that mashes up
[Plack::Middleware::Cache](https://metacpan.org/module/Plack::Middleware::Cache) and [Plack::App::Proxy](https://metacpan.org/module/Plack::App::Proxy).
