---
layout: post
title: Reading code - Plack
date: '2014-04-08'
comments: true
categories:
  - perl

---

I write lots of code.  But I want to be better and faster at reading code.
When I had the privilege of working with @ranguard I discovered he is a code
reading *cheetah* and I always envied that.  So I've decided to practice
by reading the source code of various CPAN modules.  I'm starting with Plack.

  * [**Part 1 - An Overview**](/2014/04/08/read-plack-1)
  * [Part 2 - plackup Architecture](/2014/04/09/read-plack-2)
  * [Part 3 - PSGI Application Architecture](/2014/04/11/read-plack-3)
  * [Part 4 - Plack::Builder](/2014/04/12/read-plack-4)

# Plack

Plack describes itself as a set of tools for using
[PSGI](https://metacpan.org/pod/PSGI) (the Perl Server Gateway Interface).  

The earliest release of Plack on CPAN is version 0.9000 from 2009-10-13.  There
were releases several times a week for the first **two years**.  Impressively, in
2012 releases were still happening roughly once a week.  In 2013 things seem to
have cooled and now releases happen about once a month.

The code itself is written tersely and with attention to detail.  There are
almost no comments.  In fact 43 out of 71 files have fewer than 3 comments.
Of course the code is very well written which makes comments less necessary and
nowadays there is quite a bit of excellent POD as well.  

The code also makes heavy use of callbacks (code references).  That is to say
its heavily event driven. Which makes sense given the event driven nature of web
servers.  For me this gave the code a JavaScript flavor.  Take for example
`Plack::Util::foreach` which works just like
[jQuery.each()](http://api.jquery.com/jQuery.each/) by iterating over an array
calling a code reference on each item.

    Plack::Util::foreach([1,2,3], sub { print shift }); # prints "123"


## Background reading

The most important thing to read is the [PSGI spec](https://metacpan.org/pod/distribution/PSGI/PSGI.pod). 
This is the problem Plack was built to solve.  Its clear and well
written but perhaps also a little boring and lacking in context. Still I found
it very helpful to refer back to while reading the code.

## Getting started

    ~/code ⚡ git clone git@github.com:plack/Plack.git
    ~/code ⚡ cd Plack

The first thing I noticed is a `cpanfile` containing a list of the project
dependencies.  Because understanding and running the tests is often useful
when reading new code I installed the dependencies using
[Carton](https://metacpan.org/pod/Carton) and ran the tests.

    ~/code/Plack ⚡ carton
    ~/code/Plack ⚡ prove -rl t

## Who works on Plack?

Lets get a feel for who is involved in the project.

    ~/code/Plack ⚡ git shortlog --summary --numbered | head
    1567  Tatsuhiko Miyagawa
        70  Kazuho Oku
        68  Tokuhiro Matsuno
        20  Daisuke Murase
        20  Jesse Luehrs
        19  yappo
        16  Karen Etheridge
        16  Mark Stosberg
        12  hiratara
        11  Stevan Little

## How big is it?

    ~/code/Plack ⚡ tree lib | tail -1
    17 directories, 70 files

    ~/code/Plack ⚡ cloc . 2>/dev/null | tail -13
    -------------------------------------------------------------------------------
    Language                     files          blank        comment           code
    -------------------------------------------------------------------------------
    Perl                            82           2889           3341           5309
    Bourne Shell                     8             55            138            251
    YAML                             1              0              0             19
    HTML                             3              2              0             14
    Python                           1              2              1              6
    Javascript                       1              0              0              1
    CSS                              1              0              0              1
    -------------------------------------------------------------------------------
    SUM:                            97           2948           3480           5601
    -------------------------------------------------------------------------------

## What is the lib directory structure like?

Unfortunately with 71 files in the lib directory its hard to grok whats
happening in a single glance so maybe thats not a great question.  Instead I
found it helpful to group the code by functionality.  I came up
with 3 major categories which look like this: 

  * *Category 1* - Modules for loading and running PSGI servers
    * plackup 
    * Plack::Handler
    * Plack::Handler::\*
    * Plack::Loader
    * Plack::Loader::\*
    * Plack::Runner
  * *Category 2* - Modules for building PSGI apps
    * Plack::App::\*
    * Plack::Builder
    * Plack::Component
    * Plack::Middleware
    * Plack::Middleware::\*
  * *Category 3* - Modules for testing
    * Plack::Test
    * Plack::Test::\*
    * Plack::HTTP::Message::PSGI
    * Plack::LWPish

However I also got crazy and went ahead and listed *everything* in the lib
directory along with a brief description.  I guess it might be handy for
reference purposes.  The numbers (1), (2), (3) below correspond to the three
categories I listed above.
    

    ~/code/Plack ⚡ tree lib
    lib
    ├── HTTP
    │   ├── Message
    │   │   └── PSGI.pm               # (3) Converts an HTTP::Request to a PSGI env hash
    │   └── Server                    
    │       └── PSGI.pm               # (1) Reference PSGI web server; no deps; not usually for prod
    ├── Plack                         
    │   ├── App                       # (2) These libs inherit from Plack::Component; they are PSGI web apps
    │   │   ├── Cascade.pm               # Foreach request, tries a number of PSGI apps until one is successful
    │   │   ├── CGIBin.pm                # Creates many PSGI apps for a directory with many CGI scripts (uses WrapCGI.pm)
    │   │   ├── Directory.pm             # Serves a directory of files
    │   │   ├── File.pm                  # Serves a file
    │   │   ├── PSGIBin.pm               # Create PSGI apps from a directory of .psgi files
    │   │   ├── URLMap.pm                # Maps a url to a PSGI app
    │   │   └── WrapCGI.pm               # Creates a single PSGI app from a single CGI script
    │   ├── Builder.pm                # (2) A DSL for building Plack Middleware
    │   ├── Component.pm              # (2) A (optional) tool for building PSGI web apps
    │   ├── Handler
    │   │   ├── Apache1.pm
    │   │   ├── Apache2
    │   │   │   └── Registry.pm
    │   │   ├── Apache2.pm
    │   │   ├── CGI.pm
    │   │   ├── FCGI.pm
    │   │   ├── HTTP
    │   │   │   └── Server
    │   │   │       └── PSGI.pm                 # A Plack::Handler for HTTP::Server::PSGI
    │   │   └── Standalone.pm            # Alias for Plack::Handler::HTTP::Server::PSGI
    │   ├── Handler.pm                # (1) Instantiate and run PSGI compatible servers
    │   ├── HTTPParser
    │   │   └── PP.pm                    # Parse HTTP headers with XS
    │   ├── HTTPParser.pm             # (1) Parse HTTP headers; used by HTTP::Server::PSGI
    │   ├── Loader
    │   │   ├── Delayed.pm               # Delay compilation of the PSGI app until the first request
    │   │   ├── Restarter.pm             # Restart the server when a watched file changes
    │   │   └── Shotgun.pm               # Recompile the PSGI app for every request
    │   ├── Loader.pm                 # (1) Load PSGI compatible web servers
    │   ├── LWPish.pm                 # (3) Light version of LWP for testing
    │   ├── Middleware                
    │   │   ├── AccessLog
    │   │   │   └── Timed.pm                # Write access logs but can handle a fake File::IO body
    │   │   ├── AccessLog.pm             # Write access logs
    │   │   ├── Auth
    │   │   │   └── Basic.pm                # Basic authentication
    │   │   ├── BufferedStreaming.pm     # Enable streaming for servers that don't
    │   │   ├── Chunked.pm               # Implements part of HTTP/1.1 - chunked HTTP transfer encoding
    │   │   ├── ConditionalGET.pm        # Implements part of HTTP/1.1 - Conditional GET
    │   │   ├── Conditional.pm           # Runs the specified middleware if a specified condition is met
    │   │   ├── ContentLength.pm         # Adds a Content-Length header if possible
    │   │   ├── ContentMD5.pm            # Sets the Content-MD5 header when the body is an arrayref
    │   │   ├── ErrorDocument.pm         # Show different error documents for different HTTP errors
    │   │   ├── Head.pm                  # Delete any response body from HEAD requests
    │   │   ├── HTTPExceptions.pm        # Redirect to an error page when HTTP::Exceptions are caught
    │   │   ├── IIS6ScriptNameFix.pm     # Fix for IIS
    │   │   ├── IIS7KeepAliveFix.pm      # Fix for IIS
    │   │   ├── JSONP.pm                 # Change JSON responses to JSONP if a callback param is specified
    │   │   ├── LighttpdScriptNameFix.pm # Fix for Lighttpd
    │   │   ├── Lint.pm                  # Checks input/output for compliance w/PSGI spec
    │   │   ├── Log4perl.pm              # Log with Log::Log4Perl
    │   │   ├── LogDispatch.pm           # Log with Log::Dispatch
    │   │   ├── NullLogger.pm            # Don't log anything
    │   │   ├── RearrangeHeaders.pm      # Fix for very old MSIE and broken HTTP proxy servers
    │   │   ├── Recursive.pm             # Allows the app to forward the request to a different (url) path
    │   │   ├── Refresh.pm               # Similar to Plack::Loader::Restarter but less effective
    │   │   ├── Runtime.pm               # Sets the 'X-Runtime' HTTP response header = app's response time
    │   │   ├── SimpleContentFilter.pm   # Filter response content
    │   │   ├── SimpleLogger.pm          # Logs messages
    │   │   ├── StackTrace.pm            # Displays a stacktrace when a PSGI app dies
    │   │   ├── Static.pm                # Serve static files
    │   │   ├── XFramework.pm            # Adds an X-Framework HTTP response header
    │   │   └── XSendfile.pm             # Adds an X-Sendfile HTTP response header
    │   ├── Middleware.pm             # (2) Wraps PSGI apps; can modify incoming requests / outgoing responses
    │   ├── MIME.pm                   # A list of MIME types (mostly)
    │   ├── Request
    │   │   └── Upload.pm                # A subclass of Plack::Request for file uploads
    │   ├── Request.pm                # (2) Low level request obj for middleware and web apps
    │   ├── Response.pm               # (2) Low level response obj for middleware and web apps
    │   ├── Runner.pm                 # (1) The guts of plackup -- uses Plack::Loader and Plack::Handler
    │   ├── TempBuffer.pm             # For backward compat. Saves data in memory or to a file if its big;
    │   ├── Test
    │   │   ├── MockHTTP.pm              # Test a PSGI app without using a server (faster)
    │   │   ├── Server.pm                # Test a PSGI app using a very small server (fast)
    │   │   └── Suite.pm                 # Ensure the web server complies with the PSGI spec
    │   ├── Test.pm                   # (3) A factory for generating test objects
    │   ├── Util
    │   │   └── Accessor.pm              # Light version of Class::Accessor for backward compat
    │   └── Util.pm                   # Misc but important utilities used throughout the code base
    └── Plack.pm                   # No code here -- just pod and a version number

