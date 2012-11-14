---
layout: post
title: fixing htmlfontify in scpaste
tags: emacs htmlfontify
---

While trying out [scpaste](http://github.com/technomancy/scpaste) I encountered some delightful broken by default behaviour with htmlfontify. Htmlfontify decides to try and set html font size by looking at font information from your current emacs session. Evidently this code doesn't work so well in a terminal :/ 

Luckily I found a pastebin entry somewhere (oh the irony) with this following gem.

{% highlight cl %}

(add-hook 'hfy-post-html-hooks
      (lambda ()
        ;; Replace font-size: 0pt with nothing on htmlfontify-buffer
        ;; which is called from scpaste. This happens when you're
        ;; using a console-based emacs.
        (beginning-of-buffer)
        (while (search-forward "font-size: 0pt; " nil t)
          (replace-match "" nil t))))


{% endhighlight %}



So, I figured I'd preserve it hear for other folks hit by the same issue. You know, those crazy folks that have the audacity to run emacs _in a terminal_.

