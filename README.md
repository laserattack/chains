# Wtf??

A very simple and dirty script that I use for backups

# Usage

```
ark - simple and dirty script for making backups
Usage: ark [-lpth]

Flags:
  -l, --list FILE   Backup list file (default: /home/serr/.ark/list)
  -p, --path DIR    Backup dir path (default: /home/serr/.ark/bak)
  -t, --tar         Create tar archive from backup directory
                    and put it near backup dir
  -h, --help        This help

1. RSYNC AND TAR ARE REQUIRED!!!!
2. IF THE BACKUP DIR IS INSIDE THE DIRECTORIES YOU'RE BACKING UP
(RECURSION), SOMETHING TERRIBLE CAN HAPPEN!!!!!!!!

Backup list file example:
  # exclude patterns
  !buildroot

  # add in backup (absolute paths!!)
  ~/projects
  ~/software
  ~/3dparty
  ~/.ai_key
  ~/.ark/list
  ~/.bash_history
  ~/.z
```