---
layout: post
title: Devops with Rex
date: '2013-11-22'
comments: true
categories:
  - [perl, devops]

---

I have recently been playing with [Rex](http://www.rexify.org/) and wanted to
write down some of my initial thoughts.  

Rex is a devops tool that helps you manage your servers.  Its similar to
Puppet Lab's [mcollective](http://puppetlabs.com/mcollective) tool or
[ansible](http://www.ansibleworks.com/docs/).  You can easily run commands on
your entire fleet of boxen or just on certain subgroups.  And you can run
them in parallel which saves you giant baskets of time.

The big advantage for People Who Prefer Perl (PWPP) is that Rex is written and
configured with Perl. So for the most part you don't need to spend a lot of time
learning yet another DSL.  

Another happy positive is that you don't need to install any software on your
servers. Installation is simple:

    $ apt-get install libssh2-1-dev
    $ cpanm Rex

Configuration is also simple -- at least if you know Perl.  The
Rex configuration file is named 'Rexfile' and its syntax is Perl with some
extra Rexy sugar methods thrown in. And I feel the sugar and general API for
Rex is fairly well done.  Here is an example Rexfile:

    # Configure the default user and your ssh keys.  The default user can be
    # overridden on the command line with the -u option.
    user "joe";
    private_key "/home/joe/.ssh/id_rsa";
    public_key "/home/joe/.ssh/id_rsa.pub";
    key_auth;

    # Cofigure server groups
    group prod => "webserver", "mailserver", "dbserver";
    group dev  => "pancake[1-3]", "narwhale[1-3]", "honeybadger[1-3]";

    # Run commands in parallel on up to 100 servers at one time.  This can be
    # overridden on the command line with the -t option.
    parallelism 100;

    # Create tasks.  This task runs against all servers by default.  This can
    # be overridden on the command line with the -G or -H options.
    task 'uptime', group => 'all', sub {
        my $output = run "uptime";
        say $output;
    };

You can see that Perl's 'say' command is available by default.  'run' is a Rex
sugar method which accepts a shell command and returns the output -- similar
to Capture::Tiny.

Here is a command line example which runs the 'uptime' task as the 'root' user
on each server in the 'dev' group:  

    $ rex -G dev -u root uptime
        pancake1:  16:42:05 up 221 days,  9:49,  1 user,  load average: 0.00, 0.00, 0.00
       narwhale2:  16:42:05 up 8 days,  3:28,  4 users,  load average: 0.00, 0.00, 0.00
       narwhale3:  16:42:05 up 17 days,  3:57,  2 users,  load average: 0.01, 0.01, 0.00
    honeybadger1:  16:42:05 up 80 days,  3:29,  1 user,  load average: 0.09, 0.06, 0.01
        pancake3:  16:42:05 up 137 days,  7:49,  1 user,  load average: 0.00, 0.00, 0.00
       narwhale1:  16:42:05 up 65 days,  5:30,  2 users,  load average: 0.46, 0.41, 0.37
    honeybadger3:  16:42:04 up 15 days,  4:49,  1 user,  load average: 2.00, 2.00, 2.00
        pancake2:  16:42:04 up 1 day,  2:23,  3 users,  load average: 1.38, 1.35, 1.30
    honeybadger2:  16:42:05 up 39 days,  1:24,  1 user,  load average: 0.08, 0.06, 0.01

Rex is fantastic for ad-hoc commands.  But it also has a great set of libraries
for doing much more like installing debian packages, user management,
virtualization, and managing EC2 boxes.  I have to say I'm deeply attracted to
its simplicity -- especially after dealing with Puppet's complicated and weird
DSL.  

If anyone uses Rex to manage more than 50 servers, I would love to get in touch
with you and ask a few questions.
