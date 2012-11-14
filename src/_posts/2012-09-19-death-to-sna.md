---
layout: post
title: death to sna
tags: gentoo virtual crtc
---

After much failure in getting this [virtual crtc patch](http://github.com/lisking/patches) running on gentoo, I finally decided to start throwing debug printfs straight into the Xorg intel driver. This is Xorg driver-speak for printf... 

{% highlight c %}

xf86DrvMsg(scrn->scrnIndex, X_INFO, "my debug string");

{% endhighlight %}

This yielded the finding that the init functions that turn on the virtual crtc aren't called. Reason being, sna support is on by default and the patches currently don't account for that. For now, a USE="-sna" is enough to workaround the problem. Watch this space for updates to the virtual crtc patches that work with sna turned on.

