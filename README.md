# Wtf??

A very simple and dirty script that I use for backups

# Usage

```
ark - simple and dirty script for making backups
Usage: ark [-lpth]

Flags:
  -l, --list FILE          Backup list file (default: /home/serr/.ark/list)
  -p, --path DIR           Backup dir path (default: /home/serr/.ark/bak)
  -t [PATH], --tar[=PATH]  Create tar archive. If PATH is specified,
                           archive will be created at PATH, otherwise
                           it will be placed near backup directory
  -h, --help               This help

Notes:
  1. RSYNC IS REQUIRED!!!!
  2. TAR IS REQUIRED TO USE IT WITH -t/--tar flag!!!!
  3. IF THE BACKUP DIR IS INSIDE THE DIRECTORIES YOU'RE BACKING UP
  (RECURSION), SOMETHING TERRIBLE CAN HAPPEN!!!!!!!!
  4. IF YOU RUN THE SCRIPT TWICE AT THE SAME TIME WITH THE SAME PATHS,
  YOU'RE WEIRD, MAN, DON'T DO THAT!!!!!!!!
  5. MAKE SURE YOU HAVE ENOUGH DISK SPACE BEFORE STARTING

Examples:
  ark
  ark --tar="/run/media/serr/KINGSTON/baks/bak_$(date +%y%m%dT%H%M%S).tar"

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