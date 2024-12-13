# Linux

## Find top X biggest files / folders

Files: `find / -xdev -type f -size +100M -exec du -sh {} ';' | sort -rh | head -n50`
Folders: `sudo du -abhx --exclude=/{proc,sys,dev,run,mnt} / | sort -r -h | head -n 20`

## Cleanup folder by date

Will delete all files older than 5 days: `find . -type f -mtime +5 -delete`

## Find files that don't contain pattern

This uses ripgrep.

`rg --files-without-match "AuthGuard" ./src/**/*.controller.ts`