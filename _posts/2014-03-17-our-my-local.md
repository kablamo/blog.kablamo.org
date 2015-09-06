---
layout: post
title: my vs our vs local
date: '2014-03-17'
comments: true
tags: perl

---

# The short version for the impatient

   * `my()` creates a local variable
   * `our()` creates a package variable
   * `local()` temporarily changes the local value of a global variable
   * The above is mostly true.

# The long version for the irrepressibly quixotic

## my()

`my` declares the listed variable to be local to the enclosing block, file,
or `eval`.  That is to say its *scope* is local.  This kind of variable is
known as a *lexical variable*.  Note that lexical variables are hidden from
subroutines which are called from within the enclosing block.  This is known as
*lexical scoping*.

## our()

`our` creates an alias to a *package variable*.  The alias is local to the
enclosing block, file, or `eval`.  That is to say the alias is lexically scoped
just like any lexical variable.  However a package variable belongs to a
package.  It can be accessed from anywhere if you use its fully qualified name.
Here are two examples of fully qualified package variables:

    $main::a
    %MyPackage::boop

Note that package variables are also global variables.

## local()

`local` gives temporary values to global variables.  It does not create a local
variable.  It is most commonly used when you want to locally modify a global
variable such as one of the punctuation variables.  For example:

    { 
        local $| = 1; # enable autoflush for STDOUT
        say "hi mom";
    }

`local` modifies the listed variable to be local to the enclosing block,
file, or `eval` -- AND to any subroutine called from within that block.  This
is known as *dynamic scoping*.  


# Sources

For a more complete understanding I recommend `perldoc perlfunc` and especially
`perldoc perlsub`.  Also the following links may be helpful:

 * https://metacpan.org/pod/perlfunc
 * https://metacpan.org/pod/perlsub
 * http://perlmaven.com/package-variables-and-lexical-variables-in-perl
 * http://www.perlmonks.org/?node_id=95813


