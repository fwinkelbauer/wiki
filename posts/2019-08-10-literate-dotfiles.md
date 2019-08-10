# Literate Dotfiles

The Irreal blog has recently published a post about [Managing dotfiles with
Org][irreal]. The idea is simple: Put your configuration files and other scripts
in a central (version controlled) place and use Emacs to create the "real files"
based on the content of your org file.

Here's a short example:

``` text
* Git

#+BEGIN_SRC conf :tangle "~/.gitconfig" :padline no
  [user]
    email = max@mustermann.com
    name = Max Mustermann

  [status]
    showUntrackedFiles = all

  [alias]
    lg = log --color --graph --oneline --decorate --abbrev-commit
#+END_SRC

#+BEGIN_SRC fundamental :tangle "~/.gitmessage" :padline no
  # * Why was this change necessary?
  # * How does it address the problem?
  # * Are there any side effects?
  # * What other options did you consider?
#+END_SRC

* Bash

#+BEGIN_SRC sh :tangle "~/bin/my-hello" :shebang "#!/bin/bash" :mkdirp yes
  set -euo pipefail

  echo hello
#+END_SRC
```

Executing the Emacs function `(org-babel-tangle)` will create three files:

- `~/.gitconfig`
- `~/.gitmessage`
- `~/bin/my-hello`
  - **Note:** The tangling function will (based on the shebang property) make
    this script executable. So there's no need to call `chmod +x`.

Oh and yes, I can use the same solution on Windows and on Linux. Dealing with
configuration or code in org-mode is also delightful: I can narrow the buffer to
only show a specific file and I'll even get real syntax highlighting.

One last trick, just for fun: `(org-babel-tangle)` can be called via the command
line:

``` shell
emacs --batch -l org --eval '(org-babel-tangle-file "foo.org")'
```

[irreal]: https://irreal.org/blog/?p=8227
