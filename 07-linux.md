# Linux

## Find top X biggest files / folders

Files: `find / -xdev -type f -size +100M -exec du -sh {} ';' | sort -rh | head -n50`
Folders: `sudo du -ax --exclude=/{proc,sys,dev,run,mnt} / | sort -n -r | head -n 20`