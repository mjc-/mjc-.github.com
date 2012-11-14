---
layout: post
title: daemonizing rtorrent under ArchLinux
tags: rtorrent tmux archlinux
---

Being a [collector of digital media](http://en.wikipedia.org/wiki/Pirate). I find myself running numerous instances of rtorrent to help keep different trackers separated. At some point early on, having to log in and restart them after rebooting got old, so I hacked together a template that I just search replace as new trackers come online.

{% highlight bash %}

#!/bin/bash

. /etc/rc.conf
. /etc/rc.d/functions

case "$1" in
  start)
    stat_busy "Starting rtorrent (why.cd)"
    install -m 3777 -o root -g http -d /run/rtorrent
    su why -c 'umask 002 ; tmux new -s rtorrent -d rtorrent' &> /dev/null
    if [ $? -gt 0 ]; then
      stat_fail
    else
      add_daemon rtorrent-why
      stat_done
    fi
    ;;
  stop)
    stat_busy "Stopping rtorrent (why.cd)"
    killall -u why -w -s 2 /usr/bin/rtorrent &> /dev/null
    if [ $? -gt 0 ]; then
      stat_fail
    else
      rm_daemon rtorrent-why
      stat_done
    fi
    ;;
  restart)
    $0 stop
    sleep 1
    $0 start
    ;;
  *)
    echo "usage: $0 {start|stop|restart}"
esac
exit 0

{% endhighlight %}
