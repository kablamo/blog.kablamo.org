---
layout: post
title: HTTP::Request::AsCurl
date: '2014-05-27'
comments: true
categories: perl code

---

Today, on a whim, I released a module called
[HTTP::Request::AsCurl](https://metacpan.org/pod/HTTP::Request::AsCurl) to
CPAN.  It converts an HTTP::Request object to a curl command.

    use HTTP::Request::Common;
    use HTTP::Request::AsCurl;
  
    my $request = POST('api.earth.defense/weapon1', { 
        target => 'mothership', 
        when   => 'now' 
    });
    
    say join "\n", $request->as_curl;
    # curl --dump-header - -XPOST "api.earth.defense/weapon1" \
    # --data 'target=mothership' \
    # --data 'when=now'
    
It works by injecting the `as_curl()` method into the HTTP::Request namespace.
This must be a bad idea and probably not a great bit of code to rely on in a
production environment.  But it is pretty convenient syntax for debugging a
REST API and I couldn't resist.  Thoughts, suggestions, criticism?


# -- UPDATE (2014-06-01) --

I released a new version which has a totally different user interface.  Here is
the new synopsis:

    use HTTP::Request::Common;
    use HTTP::Request::AsCurl qw/as_curl/;

    my $request = POST('api.earth.defense/weapon1', { 
        target => 'mothership', 
        when   => 'now' 
    });

    system as_curl($request);

    print as_curl($request, pretty => 1, newline => "\n", shell => 'bourne');
    # curl \
    # --request POST api.earth.defense/weapon1 \
    # --dump-header - \
    # --data target=mothership \
    # --data when=now

There are 2 major changes.  

 1. I'm no longer doing namespace injection.  I really liked the syntax, but it
    was problematic and unnecessary.
 2. The old version returned a formatted array of strings which was 
    not very useful.  The problem with a formatted string (as was helpfully
    pointed out to me) is you have worry about stuff like newlines which
    depends on the system you are targeting and shell escaping which depends on
    the shell you are targeting.  This is a can of worms.  Hopefully this new
    interface is an improvement.

