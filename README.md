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

I wrote this script to back up moderately important data to an external drive. Here's how I typically use it:

**Weekly full backup:**

```
chains -fd $KINGSTON/chains
```

**Multiple daily incremental backups:**

```
chains -id $KINGSTON/chains
```

**Verify backup integrity by restoring to a temporary location:**

```
chains -r -d $KINGSTON/chains -g "/home/serr/projects/temp/"
```

**Display all existing backup chains:**

```
chains -pd $KINGSTON/chains
```

> Note: $KINGSTON is a variable defined in my `.bashrc` pointing to the mount point of my external drive

# Installation

Just download somewhere

```
wget https://raw.githubusercontent.com/laserattack/chains/refs/heads/master/chains
```

Make it executable

```
chmod +x chains
```

And use

```
./chains
```

Optionally, you can add the script to your PATH

# Requirements

- Linux system
- Perl 5.10 or newer
- tar