---
layout: post
title: How to build a Duck Duck Go instant answer
date: '2014-02-28'
comments: true
categories: [perl, duckduckgo]

---

Instant answers are those little boxes at the top of the DuckDuckGo search
results page.  I made one for discovering international calling codes -- or
dialing codes if you prefer.

![x](/images/for-posts/2014-02-28-singapore.png)
![x](/images/for-posts/2014-02-28-down-under.png)

If you want to hack on DuckDuckGo and add your own instant answer its pretty
simple to [get started](http://duckduckhack.com).  You can use Vagrant and
VirtualBox to get a complete working dev environment.  But if you are already
running Ubuntu or OSX the following recipe is easier:

    curl http://duckpan.org/install.pl | perl 
    cpanm App::DuckPAN
    duckpan installdeps # installs dependencies from DuckPAN (not CPAN) to locallib

Next fork the repository you want to hack on.  To choose the correct repository
think about what kind of data source you are using to generate your instant answer.

   * Fork the [spice repo](https://github.com/duckduckgo/zeroclickinfo-spice)
     if you have a real time data source like a JSON web API.
   * Fork the [goodie repo](https://github.com/duckduckgo/zeroclickinfo-goodie)
     if you generate your instant answer with code and need no network access.
   * Fork the [fathead repo](https://github.com/duckduckgo/zeroclickinfo-fathead)
     if your data source can be placed in a key/value store.
   * Fork the [longtail repo](https://github.com/duckduckgo/zeroclickinfo-fathead)
     if your data source requires a full text search.

Now you can start hacking.  I'll show you how the capitalization instant answer
from the goodie repo works.  Below is the code.  I added comments to explain
things a bit.

    package DDG::Goodie::Capitalize;
    use DDG::Goodie;

    # If a DuckDuckGo search query contains any of these words at the start or
    # end of the query, the 'remainder' handler below will run.
    triggers startend => 'capitalize', 'uppercase', 'upper case';

    # This block of code is pretty much meta data describing this instant
    # answer.  Mostly it is used by https://duckduckgo.com/goodies.
    zci answer_type => "capitalize";
    primary_example_queries 'capitalize this';
    secondary_example_queries 'uppercase that';
    description 'capitalize a string';
    name 'Capitalize';
    code_url 'https://github.com/duckduckgo/zeroclickinfo-goodies/blob/master/lib/DDG/Goodie/Capitalize.pm';
    category 'conversions';
    topics 'programming';
    attribution twitter => 'crazedpsyc',
                cpan    => 'CRZEDPSYC' ;

    # This is is where the magic happens.  $_ contains the query minus the
    # trigger word.  The return value from this sub shows up on the DuckDuckGo
    # search results page as an instant answer.
    handle remainder => sub { uc ($_) };

To test your new instant answer you can launch a little web server with the following command:

    duckpan server

Then open your favorite web browser and surf to
http://0:5000/?q=capitalize+aliens+smell+better+than+dinosaurs and you should
see something like this:

![x](/images/for-posts/2014-02-28-capitalize.png)

