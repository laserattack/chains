# Wtf??

A very simple and dirty script that I use for backups

# Usage

```
ark - simple script for making backups
Usage: /home/serr/.local/bin/ark [-lptqvh]

Flags:
  -l, --list FILE   Backup list file (default: '/home/serr/.ark/list')
  -p, --path DIR    Backup dir path (default: '/home/serr/.ark/bak')
  -t, --tar         Create tar archive from backup directory
                    and put it near backup dir
  -q, --quiet       Quiet mode (no output)
  -v, --verbose     Verbose mode (with rsync --verbose output)
  -h, --help        This help

RSYNC AND TAR ARE REQUIRED!!!!

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