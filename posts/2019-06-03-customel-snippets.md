# Custom.el Snippets

My Emacs configuration contains a `custom.el` file, which should contain custom code, that is only relevant on a specific machine.
That's why this file is not tracked via git.
More and more bits of useful snippets show up in my `custom.el` file and I'd like to jot down some examples which I might expand on in the future.

## Calling Command Line Tools

This snippet is inspired by [a blog post](https://hackeryarn.com/post/cli-in-emacs/) which I've found on /r/emacs.
The elisp function calls the PowerShell function `Invoke-Formatter`, which is part of the [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) PowerShell module:

``` elisp
(defun fw/pspretty-buffer ()
  (interactive)
  (shell-command-on-region (point-min) (point-max) "powershell.exe -Command \"$script = $input | Out-String; Invoke-Formatter $script\" " t t))
```

## Org-Mode Agenda

After seeing [this post](https://www.reddit.com/r/emacs/comments/9v7ut1/screenshot_showcase_2018/), I've decided to dive into creating my own agenda.
I'm using a custom agenda view to keep track of things I have to do while I am at work.
The custom view show my scheduled tasks for the next three days, as well as all unscheduled tasks, sorted by their TODO statement (e.g. "TODO", "WAIT" or "DONE"):

``` elisp
(setq org-agenda-custom-commands
      '(("." "Overview (Custom)"
         ((agenda ""
                  ((org-agenda-span 3)
                   (org-agenda-start-on-weekday nil)
                   (org-agenda-show-future-repeats 'next)
                   (org-agenda-scheduled-leaders '("" ""))
                   (org-agenda-overriding-header "* Calendar\n")))
          (todo ""
                ((org-agenda-overriding-header "\n* Open\n")
                 (org-agenda-block-separator nil)
                 (org-agenda-sorting-strategy '(todo-state-up))
                 (org-agenda-todo-ignore-scheduled 'all)))
          ))))

(set-face-attribute 'org-agenda-structure nil :inherit 'default :height 1.25)
```

The custom headers make the agenda look like a regular org-mode file.
Enabling `(orgstruct-mode)` on the agenda buffer allows me to hide and show sections.

I haven't come around to like `org-capture`, so for now I've created this:

``` elisp
(defun fw/home ()
  (interactive)
  (delete-other-windows)
  (find-file "~/org/projects.org")
  (split-window-horizontally)
  (other-window 1)
  (org-agenda nil ".")
  (split-window-vertically)
  (other-window 1)
  (find-file (concat "~/org/" (format-time-string "%Y-%m-%d") ".org"))
  (other-window 1))

(global-set-key (kbd "<f12>") 'fw/home)
```

Pressing `F12` opens up my "home" view, which consists of my projects-overview file, my custom agenda, as well as a datestamped file which I use to keep track of unexpected issues, thoughts and ideas.
