---
layout: post
title: Use Carp::Always to fix circular dependencies
date: '2013-01-19'
comments: true
categories: perl

---

Circular dependencies happen when your library requires a library which
requires your library.  Here's an example.  Lets say you have 2 packages:  

**AlienPlanet.pm**

    package AlienPlanet;
    use Moose;
    use Dinosaurs;          # <--- Circular dependency
    sub has_dinosaurs {1}
    1;

**Dinosaurs.pm**

    package Dinosaurs;
    use Moose;
    use AlienPlanet;        # <--- Circular dependency
    sub has_rabies {1}
    1;

If you try to compile this, you get the following warning:

    ⚡ perl -c lib/AlienPlanet.pm 
    Subroutine has_dinosaurs redefined at lib/AlienPlanet.pm line 5.
    lib/AlienPlanet.pm syntax OK

In this case its obvious where the problem is.  

But if the package you included, included 25 other libraries, which included
other libraries, which included your original library -- then its harder to
figure out where the circle is.

Happily I discovered a good solution. First, modify **AlienPlanet.pm** to look
like this:

    package AlienPlanet;
    sub has_dinosaurs {1}   <-- swap
    use Dinosaurs;          <-- swap
    1;

Next, try compiling your code again but this time with
[Carp::Always](http://perladvent.org/2011/2011-12-04.html):

    ⚡ perl -MCarp::Always -c lib/AlienPlanet.pm                                                                                                            
    Subroutine has_dinosaurs redefined at lib/AlienPlanet.pm line 4.
            require AlienPlanet.pm called at lib/Dinosaurs.pm line 4
            Dinosaurs::BEGIN() called at lib/AlienPlanet.pm line 4
            eval {...} called at lib/AlienPlanet.pm line 4
            require Dinosaurs.pm called at lib/AlienPlanet.pm line 5
            AlienPlanet::BEGIN() called at lib/AlienPlanet.pm line 4
            eval {...} called at lib/AlienPlanet.pm line 4
    lib/AlienPlanet.pm syntax OK

Now you've got a stack trace and its easy to see where your problem is.  All
thats left is figuring out how to solve it. (Fast solution: use
[Class::Load](https://metacpan.org/module/Class::Load) in the Dinosaurs
package.)
