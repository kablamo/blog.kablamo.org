---
layout: post
title: Down a rabbit hole - Go-lang and its decentralized CPAN
date: '2013-11-13'
comments: true
categories:
  - [go, perl]

---

I have been in a rabbit hole for the last week.  I started by looking at Docker
([See my previous post](http://blog.kablamo.org/2013/11/13/docker/)).  It turns
out Docker is written in Go and so somehow I ended up learning
[Go](http://golang.org/).  Well "learning" is an overstatement.  I've read
about it for a few days and wrote a tiny bit of code.  

But anyway, the coolest thing about Go so far is its decentralized libraries.
*Centralized* collections of libraries have been all the rage the last few years
and it seems like all the languages have gotten hip to that -- CPAN, rubygems,
pypi, npm, etc.

But one of the biggest problems with publishing Perl modules on CPAN is how
difficult it is to do.  Sure its easy after you figure out the PAUSE website
exists, create a PAUSE account, read all the Dist::Zilla docs and some of the
source code, and write your own personalized plugin.  After that it just takes
one little command.

# How it works in Go

If you want to use some libraries in your Go code, it looks like this:

    import (
        "log" 
        "time"
        "errors"
        "github.com/dotcloud/docker"
        "github.com/fsouza/go-dockerclient"
    )

The first 3 are core libraries distributed as part of Go.  But the last two you
have to download and install yourself.  Doing that is very simple:

    export GOROOT=$HOME/go
    go get github.com/dotcloud/docker

This downloads all the dependencies too.  Of course.

The next question you will ask me is how do people learn about new libraries in
a decentralized world?  There are websites like
[http://gowalker.org/](http://gowalker.org/) which scrape GitHub's recent updates
page.  They also do this for BitBucket, Google Project Hosting, and a couple
other sites.  

This new language has shiny modern features and is very easy to get up and
running.

<pre style="font: 4px/2px monospace; color: #333; background: transparent; border: 0px; border-radius: 0; box-shadow: 0 0 0 0;">
                              ,'#@@@@@@@#+';;;''#@@@@@#,                            
                          ;@@@@#;.                    '@@@;                         
                       ;@@@:                             ,@@@                       
                     #@@.                                   +@@                     
                   @@+                                        +@;   +@@@@@#         
         ,,      +@+                               #@@@@+       @@@@#  @@       
     .@@@@@@@@' @@      .+@@@+.                 :@        @,     @@         @@      
    @@:      .@@;     ++       #+              @            @     #@         @@     
   @@         @;               @            @              @     @@         @     
  ;@         @#    ,:             '        @                @     @' :    #+    
  @         +@     ;               '        ;                  ,     @@@@;    ,@    
  @      '' @     @                 @       #                  @     '@@@@     @    
 :@     @@@@:    :                   .     ,                          @@@@     @    
 ;#     @@@@     @                   @     #  +@@@:             +     ,@@     ,@    
 :@     @@@    : ,@@@@             +     @ #@@@@@:            @      @    @;    
  @     .@@     @@@@@                  @ @@@@@@@            @      :@     @     
  @      @,     . @@@@@@@                  @ @@@@@:@            @       @    @:     
  ;@     @      . @@@@@.@.                 @ @@@@@            #       #' .@'      
   @@   '#       @@@@ ,            :     ; ,@@@@@             :       @@.       
    @@. @.       # @@@@@'            @      ;  #@#             :         @:         
     ,@@@        @  '@#.             +      @                  @         @:         
       .@         :                 '        @                '          :@         
       '#         +                 ;         '              .,           @         
       @:          @               @  @@@@@@,  @            ;,            @         
       @          @             @  @@@@@@@@.  @:        .@              @,        
       @             ;+         #,  #@@@@@@@@'    +@+::+@#                ++        
       @               ;@;,.,'@:    @@@@@@@@@@:                           :@        
       @                          #, ,@@@@@@   @                           @        
      .@                         @              '                          @        
      ;#                                       @                         @        
      ++                        :                ;                         @        
      +'                                         '                         @        
      #:                         @    ;@;@'@     @                         @      
      @,                          :+''   @  .@@@:                          @.       
      @,                                 @   :                             @.       
      @.                            ,    @   .                             @,       
      @,                            ;    @                                 @,       
      @,                            '    @                                 @:       
      #:                            :    @                                 @:       
      #;                             +  @ :  @                             @:       
      ++                              +;   ;.                              @:       
      ;#                                                                   @:       
      ,@                                                                   @:       
       @                                                                   @:       
       @                                                                   @,       
       @                                                                   @,       
       @                                                                 @,       
       @,                                                                  @,       
       +'                                                                  @,       
       :@                                                                  @:       
       .@                                                                  @:       
        @                                                                  @:       
        @                                                                  #;       
        @                                                                  #;       
        @                                                                  +@@,     
      '#@                                                                  '+  ;@.  
   +@.  @                                                                  '#     @ 
 ';     @                                                                  ;@      @
.       @                                                                  :@      #
@       @                                                                  ,@    @ @
+ @  ,@                                                                  .@@    # 
 @    ':@                                                                   +; .+ 
 ,.  @ .@                                                                   @     
   ;   .@                                                                   @       
       ,@                                                                   @       
       :@                                                                   @       
       ;#                                                                   @       
       '#                                                                   @       
       '+                                                                   @       
       +'                                                                   @       
       #;                                                                   @       
       #;                                                                   @       
       @:                                                                   @     
       @:                                                                   @.      
       @,                                                                   @,      
       @,                                                                   @,      
       @,                                                                   @,      
       @,                                                                   @,      
       @.                                                                   @.      
       @.                                                                   @     
       @.                                                                   @       
       @.                                                                   @       
       @,                                                                   @       
       @,                                                                   @       
       @,                                                                  .@       
       @,                                                                  '#       
       @:                                                                  @:       
       @:                                                                  @        
       #;                                                                  @        
       +'                                                                 ,@        
       '+                                                                 #'        
       ,@                                                                 @         
        @                                                                         
        @.                                                               @'         
        ;@                                                               @          
         @                                                              +@          
         '@                                                             @           
          @,                                                           #@           
          .@                                                           @            
           #@                                                         @:            
            @#                                                       @#             
             @+                                                     @@              
              @#                                                  .@@               
               @@@#                                              @   .#             
             ''    #                                            @      @            
            @       @                                          @        '           
           @         @                                       @@+        ;           
          :   .       @+                                  :@@+  +.    @  +          
          @  @      @, #@@;                            ;@@@,     ;    .  @          
          ' @     ':     ,@@@#:                   :+@@@@:         #    @ @          
          @ :    #           ;@@@@@@@#####@@@@@@@@@+.              @   ; .          
           @    @                  ,:;''';;:,                     @  #,           
            ;@##                                                     `             
</pre>
(the golang mascot)


