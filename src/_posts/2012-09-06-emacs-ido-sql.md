---
layout: post
title: emacs goodness for your sql needs
tags: emacs ido-mode sql
---

In my quest to take less trips outside the loving embrace of emacs, I
decided to investigate SQL integration. Fortune smiled upon me, and I
found a [post](http://rforge.org/2011/01/18/emacs-as-mysql-frontend/)
documenting how to configure some presets. The one change I really
wanted to make was to avoid having to repeat myself by defining
functions for each connection. The result:

{% highlight cl %}

(defun hey-everybody-its-sql-time (db-of-choice)
  "make with the sql action"
  (interactive (list
                (completing-read "DB: " sql-connection-alist)))
  (sql-connect-preset (intern db-of-choice)))

{% endhighlight %}

This works beautifully, although I do get asked to confirm the DB
details once per emacs instance. Not sure what's going on there,
if this gets on my nerves, I may dig deeper.
