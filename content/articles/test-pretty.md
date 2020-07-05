---
layout: post
title: Test::Pretty - because TAP is unnattractive
date: '2014-05-08'
comments: true
categories:
  - perl

---
  
[Test::Pretty](https://metacpan.org/pod/Test::Pretty) (artfully written by
the inimitable [@tokuhirom](http://tokuhirom.github.io/)) makes my tests look like this:

![x](/images/for-posts/2014-05-08-test-pretty.png)

This is especially nice when I have subtests.

![x](/images/for-posts/2014-05-08-test-pretty-subtest2.png)
![x](/images/for-posts/2014-05-08-test-pretty-subtest.png)



# How it works

I can enable Test::Pretty like this

    prove -MTest::Pretty -vlr t

But typing extra characters is not fun.  Happily
@tokuhirom also created a prove
[plugin](https://metacpan.org/pod/App::Prove::Plugin::retty) (which is included
with the Test::Pretty module) which allows me to do this:

    prove -Pretty -vlr t

Shorter but still too much typing so I created a `~/.proverc` file which contains
the following lines:

    --lib
    --verbose
    --comments
    --recurse
    -Pretty

Now I can get pretty verbose recursive (etc) tests and I only need to type this

    prove t

You can view my `~/.proverc` and more goodies in my [dotfiles repo](https://github.com/kablamo/dotfiles)).




