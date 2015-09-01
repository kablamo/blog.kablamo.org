---
layout: post
title: Reading code - Camel-Snake-Kebab
date: '2014-05-26'
comments: true
categories: perl readingcode writingcode

---

[Camel-Snake-Kebab](https://github.com/qerub/camel-snake-kebab) is a Clojure
library for word case conversions.  I have wanted to see this on CPAN for a
while so last week I finally ported it to Perl.  I even ported the test suite.
Here is a synopsis of my 
[String::CamelSnakeKebab](https://metacpan.org/pod/String::CamelSnakeKebab) Perl module:

    use String::CamelSnakeKebab qw/:all/;

    lower_camel_case('flux-capacitor')
           # Returns 'fluxCapacitor'

    upper_camel_case('flux-capacitor')
           # Returns 'FluxCapacitor'

    lower_snake_case 'ASnakeSlithersSlyly'    
           # Returns 'a_snake_slithers_slyly'

    upper_snake_case('ASnakeSlithersSlyly')
           # Returns 'A_Snake_Slithers_Slyly'

    constant_case("I am constant")
        # Returns "I_AM_CONSTANT"

    kebab_case('Peppers_Meat_Pineapple')
     # Returns 'peppers-meat-pineapple'

    http_header_case("x-ssl-cipher")
           # Returns "X-SSL-Cipher"

# Clojure

This was my first contact with Clojure and I found the code I was reading to be
bite sized, concise, elegant code.  It reads a bit like math equations to
me.  Or sort of vaguely BNF-like as you will see.  I suspect I would have had a
hard time choosing a better library as my introduction to the language.  

# Functional programming

Clojure is a functional language -- as contrasted with more common imperative
languages.  I will admit I didn't really know what that means.  But hey I
looked it up so I can now present to you 3 central concepts of functional
programming.  (Btw Perl is usually imperative but it can be written
functionally as well.  Although its a little easier and more natural in
Clojure).

## 1. First class and higher order functions

These are functions which accept other functions as arguments.  So functional
programmers enjoy passing around code refs.  Thats not radical for Perl
developers.  Perl has always had excellent support for that.  

This flavor of code is often shorter, more general, and less repetitive.
But its harder to read and requires me to use my brains causes me some
discomfort.

### Example 

The most important function in Camel-Snake-Kebab is `convert-case`.  It is
called by every case conversion function in the library.  It splits a string
into words, applies a case rule to the first word and then a second possibly
different case rule to the remaining words.  Then all the words are joined back
together using the given separator.  Here it is written in Clojure:

    (defn convert-case [first-fn rest-fn sep s]
    "Converts the case of a string according to the rule for the first
    word, remaining words, and the separator."
    (let [[first & rest] (split s word-separator-pattern)]
        (join sep (cons (first-fn first) (map rest-fn rest)))))

Using this I could implement lower snake case like this:

    (defn lower-snake-case [s]
        (convert-case lower-case lower-case "_" s))

Here is the translation in Perl I came up with:

    sub convert_case {
        my ($first_coderef, $rest_coderef, $separator, $string) = @_; 
    
        my ($first, @rest) = split $WORD_SEPARATOR_PATTERN, $string;
    
        my @words = $first_coderef->($first);
        push @words, $rest_coderef->($_) for @rest;
    
        return join $separator, @words;
    }

    sub my_lc { lc $_ }
    
    sub lower_snake_case {
        convert_case( \&my_lc, \&my_lc, "_", shift );
    }

The cool thing about this is the different case methods (lower_camel_case,
kebab_case, etc) are not actually implemented this way.  They are dynamically
created when the module loads using a set of conversion rules that looks like
this:

    our %CONVERSION_RULES = (
        'lower_camel_case' => [ \&lc,               \&ucfirst,          ""  ],
        'upper_camel_case' => [ \&ucfirst,          \&ucfirst,          ""  ],
        'lower_snake_case' => [ \&lc,               \&lc,               "_" ],
        'upper_snake_case' => [ \&ucfirst,          \&ucfirst,          "_" ],
        'constant_case'    => [ \&uc,               \&uc,               "_" ],
        'kebab_case'       => [ \&lc,               \&lc,               "-" ],
        'http_header_case' => [ \&http_header_caps, \&http_header_caps, "-" ],
    );


## 2. Purely functional functions

These are functions with no state and no side effects.  In functional
programming I can't do assignments because that alters state (and that is a
side effect).  Which sounds rediculous.  How can I program without doing
assignments?  I'm not sure, but the advantage of no side effects is
performance.  I can run functions in parallel without affecting each other.
Also the function's output will depend entirely on the input which makes purely
functional functions great for [memoization](https://metacpan.org/pod/Memoize).

### Example

This concept was also evident in the code I ported.  Functions did not modify
state.  There were very few if any assignments.  And the output of functions
depended entirely on the input.

I did try memoizing String::CamelSnakeKebab but it did not make it faster.  I'm
not sure why.  Perhaps case conversion is just not computationally intensive
enough to make a difference?  So unfortunately I have no example for you.  Any
help in the comments would be awesome.  

## 3. No `for` loops

`for` loops require state and assignments.  To implement loops in functional
programming I'm supposed to use recursion.  I didn't see any examples of this
in this library and my brain is grateful to the author for sparing me the
exertion.

# The End

Thats the end of my story today.  If you are interested, compare the
[Perl source code](https://github.com/kablamo/perl-string-camelsnakekebab/blob/master/lib/String/CamelSnakeKebab.pm)
with the
[Clojure source code](https://github.com/qerub/camel-snake-kebab/blob/stable/src/camel_snake_kebab.clj).
Each version is about 70 lines of code.
