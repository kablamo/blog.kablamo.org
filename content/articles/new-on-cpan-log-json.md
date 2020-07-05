---
layout: post
title: New on CPAN - Log::JSON
date: '2012-04-29'
comments: true
categories:
  - perl

---

I released Log::JSON v0.001 to CPAN today.  Its a very simple JSON logger.

The advantage of a JSON logger is that a human can open a mysterious new log
file and quickly decipher the content because each piece of information is
labeled.  Using JSON also means that parsing the log file and reviving the data
structures is trivial.

Here is some example usage for you:

    use Log::JSON;
    my $logger = Log::JSON->new(
        file            => '/path/errorlog.json', # required
        date            => 1, # optional
        remove_newlines => 1, # optional
    );
    $logger->log(a => 1, b => 2);
    # '/path/errorlog.json' now contains:
    # {"__date":"2010-03-28T23:15:52Z","a":1,"b":1}

I wish I had written it as a Log::Dispatch plugin, and perhaps I'll get around
to that sometime.

One problem with using JSON is that there is a lot repetition and if your log
file is a bajillion lines long, then that's going to be a big file.  Happily,
file compression solves this problem very well.  And Vim and less handle
compressed files on the fly so viewing the file is not inconvenient.  And now
that I write this, I think some kind of compression feature may be nice for
Log::JSON and pretty great for Log::Dispatch too.

Log::JSON on github: https://github.com/kablamo/Log-JSON

