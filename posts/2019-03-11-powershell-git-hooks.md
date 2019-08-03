# Use PowerShell Scripts in Git Hooks

Adding a small git hook into a project is rather easy. The following hook calls
a PowerShell script which in turn runs a [psake][psake]
task to search for linter issues. The script (`./.git/hooks/pre-push`) looks
like this:

``` shell
#!/bin/sh

C:/Windows/System32/WindowsPowerShell/v1.0/powershell.exe -Command 'psake Invoke-Linter'
```

**Warning:** Hooks should not contain carriage returns. Use `LF` instead of
`CRLF`.

And here's a tiny psake task which helps me to install git hooks:

``` powershell
task Install-GitHooks {
    Get-ChildItem '.\my_hooks' | Copy-Item -Destination '.\.git\hooks' -Force
}
```

[psake]: https://github.com/psake/psake
