# My Emacs Journey 2018

I have started to use Emacs around the summer time after shelving the task of
getting started for several weeks. My intention was to find a cross-platform
editor which is not Electron-based (e.g. Atom or Visual Studio Code). The
thought of learning Emacs, an editor which is pretty much older than I am,
seemed intimidating at first, which was the major reason for putting it off for
so long. Now, at the start of the new year, I can look back and say, that the
invested time was well spent. I'd like to summarize a few concepts and tools
that I have learned along the way.

## The Right Tool for the Right Job

I am still using other applications such as Notepad++ or Visual Studio and that
is fine. I don't feel the need to use Emacs as a hammer for every nail or screw
that I'll encounter.

## New Ways of Thinking

Emacs is different than most editors or IDEs that I have used in the past.
Concepts such as buffer management, the "everything is text" philosophy or the
tinkering aspects were rather new to me. It took me a few days to internalize
the common vocabulary (e.g. `M-x foobar`, minibuffer, killing and yanking, well
known package names...), which made re-reading several resources pretty
important. The [/r/emacs][reddit] subreddit has helped me a lot.

I intentionally decided to start with the plain vanilla version of GNU Emacs so
that I could start with the "original" way of how to use to editor. Pretty soon
some aspects started to itch, which is where I either had to learn the "intended
way" or I had to bring in some external packages (extensions).

## Org Mode

Most of my personal or work related notes used to be written in Markdown. I have
since migrated a major chunk of my personal notes to org-mode. The hierarchical
and task-oriented nature of org-mode really appeals to me. I do not consider
myself a power user, because I am only using a few of org-mode's features, e.g.
I still prefer a traditional calendar, even if org-agenda has some appealing
features.

You can find my literate Emacs configuration [here][github]. Even though one
would consider committing files managed by a package manager a no-go, I have
decided to add all `.el` and `*.elc` files, so that I can get the exact same
code when checking out the repository on a new computer.

## Magit

Magit was one of the reasons why I have decided to learn Emacs in the first
place. I still like to fire up the good old console to use the git CLI, but
because of its interactive nature, Magit has made the process of staging and
committing files fun.

## Ivy, Counsel and Swiper

Emacs ships with ido, a completion framework that you can turn on to e.g. speed up
the process of finding files in the minibuffer. I did not enjoy the look and
feel of ido, which is why I turned to ivy. I have never tried helm (ivy and helm
seem to be the two "go-to" completion packages), which is why I can't give an
educated insight into their differences, but from what I have read, ivy seems to
be more "light weight" compared to helm.

IDEs do offer features such as finding and replacing text in different files,
but ivy offers these features in a way, that I have never seen before. I really
enjoy the quick feedback and interactive nature that ivy can pull off.

## Elfeed

I have recently re-discovered RSS feeds for myself. Back in the day, I was a
heavy Google Reader user, but those days are long gone. Elfeed is a decent RSS
reader that I have started to enjoy. Based on some custom configuration from the
author himself, I have written my own elisp functions which integrate the
youtube-dl CLI to download podcasts and videos in my feed.

## Further Improvements and Closing Thoughts

It might seem crazy that the process of learning and configuring Emacs is never
really "done", but most of the time it does not really feel like work.

Things that I might tinker on in the future include:

- Window management
- In-buffer navigation
- Customizing the mode line
- Creating a simple theme

[reddit]: https://reddit.com/r/emacs
[github]: https://github.com/fwinkelbauer/emacs
