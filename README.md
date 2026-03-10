![chains](chains.jpg)

# Wtf??

Simple script for backups

# Usage

```
chains - incremental backups without any bullshit
Usage: chains [-hmifrwCpv]

Flags:
  -h, --help                 This help
  -m, --main-directory DIR   Main program directory (default: ~/.chains)
  -i, --incremental          Make incremental backup (to the last chain)
  -f, --full                 Make full backup (new chain)
  -r, --recover [TIMESTAMP]  Restore the chain to the specified timestamp
                             If TIMESTAMP omitted, recovers latest state
                             TIMESTAMP format: YYMMDDTHHMMSS (e.g. 250309T143045)
  -w, --verify [TIMESTAMP]   Verify integrity of chain up to specified timestamp
                             If TIMESTAMP omitted, verifies entire latest chain
                             TIMESTAMP format: YYMMDDTHHMMSS (e.g. 250309T143045)
  -C, --directory DIR        Change to DIR before any operation
                             If DIR omitted, goes to HOME directory
  -p, --print                Print all backup chains structure
  -v, --verbose              Verbosely list files processed

Notes:
  1. GNU TAR IS REQUIRED
  2. Interrupting backup creation may leave incomplete archives
  3. Always verify your backups can be restored
  4. EXCLUDE BACKUP DIR FROM BACKUP
```

# Examples

This script was written for back up moderately important data to an external drive. Here's how I typically use it:

**Weekly full backup:**

```
chains -fm $KINGSTON/chains/
```

**Daily incremental backups (run as often as needed):**

```
chains -im $KINGSTON/chains/
```

**Verify backup integrity:**

```
chains -wm $KINGSTON/chains/
```

**Restore latest backup:**

```
chains -rm $KINGSTON/chains/ -C ~/projects/temp/
```

**Restore concrete backup using timestamp:**

```
chains -r 260309T205505 -m $KINGSTON/chains/ -C ~/projects/temp/
```

**Display all existing backup chains:**

```
chains -pm $KINGSTON/chains/
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

# Notes

When you run **chains**, it automatically checks and creates (if missing) the required directory structure in your main directory (default: `~/.chains` or custom path with `-m` flag):

```
MAIN_DIR/
├── include_paths.txt    # List paths to backup (one per line, absolute paths only)
├── exclude_patterns.txt # Patterns to exclude (tar format)
└── chains/              # All backups stored here
```

# Requirements

- Linux system
- Perl 5.10 or newer
- GNU tar