---
layout: post
title: emacs goodness for your puppet linting needs
tags: emacs puppet puppet-lint
---

One of the emacs features that first impressed me was the ability to navigate source files based on gcc compilation errors. After doing some puppet dev for a while, I decided to investigate how hard it would be to do this with puppet source and puppet-lint. Turns out, not that hard. 

My resulting init-puppet-mode.el:

{% highlight cl %}

(require 'compile)
(add-hook 'puppet-mode-hook
          (lambda ()
            (set (make-local-variable 'compile-command)
                 ;; puppet-lint current file
                 (let ((file (file-name-nondirectory buffer-file-name)))
                   (format "%s %s" "puppet-lint --with-filename" file)))))

(add-hook 'puppet-mode-hook
          (lambda ()
            (define-key puppet-mode-map (kbd "C-c C-c") 'compile)
            (define-key puppet-mode-map (kbd "C-c C-n") 'next-error)))

(add-to-list 'compilation-error-regexp-alist 'puppet)
(add-to-list 'compilation-error-regexp-alist-alist
             '(puppet "^\\(.+?\\) - \\(.+?\\): .+? on line \\([0-9]+\\)$"
                      1 3))

{% endhighlight %}

