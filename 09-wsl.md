# WSL

## Increase number of open file handles

https://github.com/microsoft/WSL/issues/1688#issuecomment-532767317

TL;DR: edit `/etc/security/limits.conf` then start a new shell: `su julien --shell /bin/zsh`. Limit is only active in the new shell (`ulimit -n`)