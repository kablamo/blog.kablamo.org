---
layout: post
title: Test::WWW::Selenium::More
date: '2012-05-17'
comments: true
tags: perl

---

I recently released
[Test::WWW::Selenium::More](https://metacpan.org/module/Test::WWW::Selenium::More)
to CPAN.  It is a small collection of utilities to help you write Selenium
tests.  Here are some reasons to use it:

1. It has a
[manual](https://metacpan.org/module/Test::WWW::Selenium::More::Manual) which
provides a concise but fairly comprehensive howto guide to Selenium testing in
Perl.

2. It uses Moose so you can more easily use roles.  For example you might want
a role for methods that deal with authentication and a role for methods that
deal with payments.

3. Smarter testing with methods like wait\_for\_jquery() and jquery\_click().  You
should never sleep() in your Selenium tests because that leads to slow tests
with random failures which leads to frustration, low morale, hair pulling, and
heavy drinking.

4. Method chaining.  Here is what this looks like:

        use Test::Most;
        use Test::WWW::Selenium::More;

        Test::WWW::Selenium::More->new()

        ->note('Search google')
        ->open_ok("http://www.google.com")
        ->title_like(qr/Google Search/)
        ->type_ok('cat pictures')
        ->follow_link_ok('Search')

        ->note('Check the number of results')
        ->is_text_present_ok('2 bajillion results');

        done_testing;

Bugs or patches?  https://github.com/kablamo/Test-WWW-Selenium-More
