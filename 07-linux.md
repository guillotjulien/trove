# Linux

## Find top X biggest files / folders

Files: `find / -xdev -type f -size +100M -exec du -sh {} ';' | sort -rh | head -n50`
Folders: `sudo du -abhx --exclude=/{proc,sys,dev,run,mnt} / | sort -r -h | head -n 20`

## Cleanup folder by date

Will delete all files older than 5 days: `find . -type f -mtime +5 -delete`

## Find files that don't contain pattern

This uses ripgrep.

`rg --files-without-match "AuthGuard" ./src/**/*.controller.ts`

## SSH into server via bastion host

Put this in your `~/.ssh/config`:
```
Host bastion
        Hostname <BASTION_HOSTNAME>
        User <BASTION_USER>
        IdentityFile ~/.ssh/<BASTION_SSH_KEY>

Host internal-server
        Hostname <INTERNAL_SERVER_HOSTNAME>
        User <INTERNAL_SERVER_USER>
        IdentityFile ~/.ssh/<INTERNAL_SERVER_SSH_KEY>
        ProxyCommand ssh -W %h:%p bastion
```

**Note**: `INTERNAL_SERVER_SSH_KEY` needs to be present on the bastion server.

Kudo to https://unix.stackexchange.com/a/41494
