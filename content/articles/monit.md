---
layout: post
title: Monit - How to know when your web site is down
date: '2013-11-24'
comments: true
categories:
  - [perl, devops]

---

If your website goes down, you want to find out fast.  There are a few ways to
accomplish this, but I'm using [monit](http://mmonit.com/monit).  Monit is a
mature unix monitoring daemon and it gives me the ability not only get alerts
but to restart services that go down. 

Monit has lotsa power and options and you can read about all of them on
the man page.  Or if you don't want to mess around with that you can pay them
for a [pretty web admin interface](https://mmonit.com/screenshots/) and some
phone/tablet apps for a one time fee of € 65.  Another simpler and I think less
powerful option is [monitor.us](http://monitis.com/?affiliate=1303230640).

I don't want to pay.  So I installed monit like this:

    apt-get install monit

The configuration file for monit lives at `/etc/monit/monitrc`.  You probably
don't need to bother with that.  When monit runs, it looks in
`/etc/monit/conf.d` and executes any scripts it finds in there.  Like many
people I'm running nginx in front of my Starman web apps.  So I want monitor
both of those processes.

Here is how to do that.  Created a script named `/etc/monit/confd/nginx`:

    check process nginx with pidfile /var/run/nginx.pid

        start program = "/etc/init.d/nginx start"
        stop  program = "/etc/init.d/nginx stop"

        alert kablamo@example.com with mail-format {
               from: monit@example.com
            subject: monit alert: $SERVICE $EVENT $DATE
            message: $DESCRIPTION
        }

        if failed port 80 protocol HTTP
            request /
            with timeout 7 seconds
            then restart

Then create a second script named `/etc/monit/conf.d/mywebapp`.  Its very
similar.  This assumes you are running your web app as the user `web` on
localhost port 22222.

    check process mywebapp with pidfile /var/run/mywebapp.pid

        start program = "/etc/init.d/mywebapp start" as uid web and gid web
        stop  program = "/etc/init.d/mywebapp stop"  as uid web and gid web

        alert kablamo@example.com with mail-format {
               from: monit@example.com
            subject: monit alert: $SERVICE $EVENT $DATE
            message: $DESCRIPTION
        }

        if failed port 22222 protocol HTTP
            request /
            with timeout 7 seconds
            then restart

With these scripts, anytime your processes disappear or stop working you will
get email and monit will try to restart them. 

But I'm paranoid.  So I created a third monit script to do an end to end test in case
something ever gets misconfigured somewhere. `/etc/monit/conf.d/end2end`:

    check host networthify.com with address 71.19.156.131
            
        alert kablamo@example.com with mail-format {
               from: monit@monit@example.com
            subject: monit alert: $SERVICE $EVENT $DATE
            message: $DESCRIPTION
        }

        if failed port 80 protocol HTTP
            request /
            with timeout 9 seconds
            then alert


Suggestions for improvment?


