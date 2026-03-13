![chains](chains.jpg)

# Wtf??

Simple script for backups

Why another backup tool? Because most existing solutions are way too complicated. They offer a ton of shit like support for dozens of protocols, encryption, rotation policies, and worst of all - they store your data in their own format. Good luck restoring anything if you don't have the binary on hand

I wanted something **dumb and simple**. A tool that just does incremental backups and nothing else. No dependencies, no magic. Just plain `tar.gz` files that you can access on any Linux system with nothing but `tar`

This entire script is essentially a convenient interface to `tar`

# Usage

```
chains - incremental backups without any bullshit

Usage: chains [OPTION]...

Options:
  -h, --help                 This help

  -m, --main-directory DIR   Main program directory (default: ~/.chains)

  -i, --incremental          Make incremental backup (to the last chain)

  -f, --full                 Make full backup (new chain)

  -r, --recover [TIMESTAMP]  Restore the chain to the specified timestamp
                             TIMESTAMP: YYMMDDTHHMMSS | latest
                             If TIMESTAMP omitted, recovers latest state

  -w, --verify [RANGE]       Verify integrity of chain
                             TIMESTAMP: YYMMDDTHHMMSS | latest
                             RANGE: TIMESTAMP | TIMESTAMP:TIMESTAMP
                             Examples: 250309T143045
                                       latest
                                       250309T143045:250310T102030
                                       250309T143045:latest
                                       latest:latest
                             If RANGE omitted, verifies the chain
                             up to the latest state

  -C, --directory DIR        Change to DIR before any operation

  -p, --print                Print all backup chains structure

  -v, --verbose              Verbosely list files processed

  -y, --yes                  Automatically answer yes to all prompts

  -n, --no                   Automatically answer no to all prompts
                             Overrides --yes

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

**Verify integrity of backup chain (check if all archives in chain/subchain can be unpacked):**

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

# How it works

When you run **chains**, it automatically checks and creates (if missing) the required directory structure in your main directory (default: `~/.chains` or custom path with `-m` flag):

```
MAIN_DIR/
├── include_paths.txt
├── exclude_patterns.txt
└── chains/

* include_paths.txt - absolute paths to backup (one per line)
* exclude_patterns.txt - tar exclusion patterns (one per line)
* chains/ - directory containing all backup chains
```

A **chain** is a sequence of backups (`tar.gz` archives), where the first backup is full and all subsequent backups are incremental (containing only changes since the previous backup).

## Starting a new chain

To start a new chain, create a full backup with:

```
chains -fm /path/to/main/dir
```

**chains** will create a folder for the chain and place the first backup there:

```
MAIN_DIR/
├── chains
│   └── since_260309T200625
│       ├── [2.0G] 260309T200625.tar.gz
│       └── incremental.snar
├── exclude_patterns.txt
└── include_paths.txt

* chains/ - directory containing all backup chains
* since_260309T200625/ - specific chain directory (named after start time)
* .tar.gz files - backup archives (first is full, rest are incremental)
* incremental.snar - GNU tar metadata for tracking changes
* exclude_patterns.txt - tar exclusion patterns (one per line)
* include_paths.txt - absolute paths to backup (one per line)
```

## Adding incremental backups

Once a chain exists, add incremental backups with:

```
chains -im /path/to/main/dir
```

After the first incremental backup, the chain folder will look like this:

```
MAIN_DIR/
├── chains
│   └── since_260309T200625
│       ├── [2.0G] 260309T200625.tar.gz
│       ├── [1.2M] 260309T202739.tar.gz
│       └── incremental.snar
├── exclude_patterns.txt
└── include_paths.txt

* chains/ - directory containing all backup chains
* since_260309T200625/ - specific chain directory (named after start time)
* .tar.gz files - backup archives (first is full, rest are incremental)
* incremental.snar - GNU tar metadata for tracking changes
* exclude_patterns.txt - tar exclusion patterns (one per line)
* include_paths.txt - absolute paths to backup (one per line)
```

As you continue taking incremental backups, they are added to the same chain:

```
MAIN_DIR/
├── chains
│   └── since_260309T200625
│       ├── [2.0G] 260309T200625.tar.gz
│       ├── [1.2M] 260309T202739.tar.gz
│       ├── [1.2M] 260309T202747.tar.gz
│       ├── [1.5M] 260309T205505.tar.gz
│       ├── [1.4M] 260309T211003.tar.gz
│       ├── [1.7M] 260309T220534.tar.gz
│       ├── [1.2M] 260309T221151.tar.gz
│       ├── [1.5M] 260310T131826.tar.gz
│       ├── [1.5M] 260310T133139.tar.gz
│       ├── [221M] 260310T133328.tar.gz
│       ├── [1.3M] 260310T133457.tar.gz
│       ├── [1.4M] 260310T135812.tar.gz
│       ├── [1.6M] 260310T153300.tar.gz
│       ├── [1.3M] 260310T153454.tar.gz
│       ├── [1.3M] 260310T153504.tar.gz
│       ├── [1.3M] 260310T153509.tar.gz
│       ├── [1.6M] 260310T154758.tar.gz
│       ├── [1.9M] 260310T164949.tar.gz
│       └── incremental.snar
├── exclude_patterns.txt
└── include_paths.txt

* chains/ - directory containing all backup chains
* since_260309T200625/ - specific chain directory (named after start time)
* .tar.gz files - backup archives (first is full, rest are incremental)
* incremental.snar - GNU tar metadata for tracking changes
* exclude_patterns.txt - tar exclusion patterns (one per line)
* include_paths.txt - absolute paths to backup (one per line)
```

When you want to start a fresh chain (for example, to create a new full backup baseline), simply run:

```
chains -fm /path/to/main/dir
```

This creates a new timestamped folder (e.g., `since_260310T170000`) and places a new full backup there. All subsequent incremental backups with `-i` will be added to this newest chain.

## Restoring data

To restore your data to a state corresponding to a specific backup in a chain, you need to extract all backups from the first (full) backup up to and including the target backup, extracting them all into the same directory. This applies the changes sequentially, recreating the exact state at that point in time.

You can do this manually, or use **chains**:

```
# Restore to the latest backup
chains -rm /path/to/main/dir -C /path/to/output/dir

# Restore to a specific timestamp
chains -r 260309T205505 -m /path/to/main/dir -C /path/to/output/dir
```

# Requirements

- Linux system
- Perl 5.10 or newer
- GNU tar

# References

- [GNU tar manual](https://www.gnu.org/software/tar/manual/html_node/tar_toc.html)
- [stackoverflow.com/questions/2001709/how-to-check-if-a-unix-tar-gz-file-is-a-valid-file-without-uncompressing](https://stackoverflow.com/questions/2001709/how-to-check-if-a-unix-tar-gz-file-is-a-valid-file-without-uncompressing)