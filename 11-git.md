# Git

## Current gitconfig

```
[core]
  editor = nano

[push]
  # Setup upstream automatically - needs git >= 2.37 to work
  default = simple
  autoSetupRemote = true

[column]
  ui = auto

[branch]
  # Sort branch by last commit date
  sort = -committerdate

[tag]
  # Sort tags in correct alphabetical order
  sort = version:refname

[init]
  defaultBranch = main

[diff]
  algorithm = histogram
  colorMoved = plain

[help]
  autocorrect = prompt

[commit]
  # Show diffs when committing
  verbose = true

[rerere]
  enabled = true
  autoupdate = true
```

Thanks https://blog.gitbutler.com/how-git-core-devs-configure-git/