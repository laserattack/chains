# Wtf??

Simple script that I use for backups

# Usage

```
chains - simple script for making incremental backups
Usage: chains [-hpifrgd]

Flags:
  -h, --help                 This help
  -p, --print                Print all backup chains structure
  -i, --incremental          Make incremental backup
  -f, --full                 Make full backup
  -r, --recover [TIMESTAMP]  Recover backup chain to current dir.
                             If TIMESTAMP omitted, recovers latest state.
                             TIMESTAMP format: YYMMDDTHHMMSS (e.g. 250309T143045)
                             Use 'latest' explicitly if needed (default when omitted)
  -g, --goto DIR             Change to DIR before any operation
                             If DIR omitted, goes to HOME directory
  -d, --dir DIR              Backup dir (default: /home/serr/.chains)

Notes:
  1. TAR IS REQUIRED
  2. Interrupting backup creation may leave incomplete archives
  3. Always verify your backups can be restored
  4. EXCLUDE BACKUP DIR FROM BACKUP
```

# Examples

I wrote this script to back up moderately important data to an external hard drive. Here's how I typically use it:

**Weekly full backup:**

```
chains --dir "/run/media/serr/KINGSTON/.chains" --full
```

**Multiple daily incremental backups:**

```
chains --dir "/run/media/serr/KINGSTON/.chains" --incremental
```

**Verify backup integrity by restoring to a temporary location:**

```
chains --dir "/run/media/serr/KINGSTON/.chains" --goto "/home/serr/projects/temp/" --recover
```

**Display all existing backup chains:**

```
chains --dir "/run/media/serr/KINGSTON/.chains" --print
```