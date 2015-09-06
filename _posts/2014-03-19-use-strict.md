---
layout: post
title: What does 'use strict' do?
date: '2014-03-19'
comments: true
tags: perl

---

I always `use strict` in all my code.  But what does that actually mean?

# Enables strict refs

Strict refs generates runtime errors when you use symbolic references.  

    use strict 'refs';
    $ref = "yarrr matey";
    print $$ref;        # runtime error but without strict refs this is ok

# Enables strict vars

Strict vars generates a compile time error if you access a variable that was
not declared or is not fully qualified.

    use strict 'vars';
    $X::foo = 1;         # ok because its fully qualified
    my $foo = 10;        # ok because my() was used.
    $baz = 9;            # compile time error because $baz not declared before

# Enables strict subs

Strict subs generates a compile time error if you use a bareword identifier
that's not a subroutine.

    no strict 'subs';
    my $a = boop;
    print $a; 
    sub boop { return "dinosaurs smell good" }

The above prints "boop" instead of "dinosaurs smell good".

    use strict 'subs';
    my $b = splarf;      # <--- compile time error here
    sub splarf { return "dinosaurs smell good" }

The above generates a compile time error on line 2.

    use strict 'subs';
    my $b = splarf(); 
    sub splarf { return "dinosaurs smell good" }

The above prints "dinosaurs smell good" which is probably the desired output.


Sources: 

 * https://metacpan.org/pod/strict
 * http://www.perl.com/pub/2001/01/begperl6.html#use strict

