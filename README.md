# xSHELL

English | [中文](README.zh.md)

`xSHELL` is a small helper collection for daily Shell work. It wraps frequently used but easy-to-forget commands into a stable format:

```bash
xSHELL <command> [options]
```

Current version: `0.2.0`

Examples:

```bash
xSHELL ext -e tsv -m .
xSHELL find '*.fa*' -s size -r .
xSHELL ren -e fa:fasta *.fa
xSHELL top -u USER -s mem
```

## Installation

From this directory:

```bash
chmod +x xSHELL
```

Then add this directory to `PATH`, or create a symlink from a directory already in `PATH`.

Example:

```bash
ln -s /path/to/xSHELL/xSHELL ~/bin/xSHELL
```

If `~/bin` is not in `PATH`, add this to `~/.zshrc` or `~/.bashrc`:

```bash
export PATH="$HOME/bin:$PATH"
```

## Help

List all commands:

```bash
xSHELL list
```

Show the version:

```bash
xSHELL version
xSHELL -v
```

Show general help:

```bash
xSHELL help
```

Show help for one command:

```bash
xSHELL help ext
xSHELL help find
xSHELL help path
xSHELL help size
xSHELL help top
```

## Command Summary

Current commands:

```text
ext
find
path
ren
size
top
version
```

Meanings:

| Command | Meaning |
| --- | --- |
| `ext` | Check child directories by file extension, such as which directories contain or miss `tsv`, `fasta`, `fa`, or `vcf` files |
| `find` | Recursively find files or directories and sort by name, size, or modified time |
| `path` | Store and print frequently used paths, such as genome FASTA paths |
| `ren` | Rename files in batch, with preview by default |
| `size` | Show disk usage for immediate child directories |
| `top` | Show process snapshots, such as tasks from one user, one PID, or processes sorted by CPU or memory |
| `version` | Print the installed xSHELL version |

## ext

`ext` checks immediate child directories and reports whether they contain files with a given extension.

Show child directories that contain `*.tsv` files:

```bash
xSHELL ext -e tsv .
```

Show child directories that do not contain `*.fasta` files:

```bash
xSHELL ext -e fasta -m .
```

Show both statuses for `*.vcf` files:

```bash
xSHELL ext -e vcf -b .
```

Only check files directly inside each child directory:

```bash
xSHELL ext -e fa -d .
```

Notes:

- `-e tsv` or `--ext tsv` checks `*.tsv`
- `-e fasta` or `--ext fasta` checks `*.fasta`
- `--has` prints directories containing matching files
- `-m` or `--missing` prints directories not containing matching files
- `-b` or `--both` prints both groups
- `-d` or `--direct` only checks files directly inside each child directory
- By default, each child directory is checked recursively

## find

`find` recursively finds files or directories and can sort results by name, file size, or modified time.

Find all `*.tsv` files:

```bash
xSHELL find '*.tsv' .
```

Find FASTA-like files and sort by size, largest first:

```bash
xSHELL find '*.fa*' -s size -r .
```

Find files whose names contain `genome`, sorted by modified time, newest first:

```bash
xSHELL find '*genome*' -s time -r .
```

Find directories named `results`:

```bash
xSHELL find 'results' --type d .
```

Common options:

- `-s name` or `--sort name` sorts by name, the default
- `-s size` or `--sort size` sorts by file size
- `-s time` or `--sort time` sorts by modified time
- `-r` or `--reverse` reverses the order, such as largest first or newest first
- `--type f` finds files only, the default
- `--type d` finds directories only
- `--type a` finds all path types

## path

`path` stores and prints frequently used paths. It is useful for long genome, annotation, and project paths.

Print the saved `ref-genome` path:

```bash
xSHELL path ref-genome
```

The default database includes:

```text
ref-genome    /data/reference/genome.fa
```

Add or update a path:

```bash
xSHELL path add ref /data/reference/genome.fa 'reference genome fasta'
```

List saved paths:

```bash
xSHELL path ls
```

Remove a saved path:

```bash
xSHELL path rm ref
```

Show the path database location:

```bash
xSHELL path db
```

The default path database is:

```text
~/.config/xSHELL/paths.tsv
```

## ren

`ren` renames files in batch. It previews the rename plan by default. Add `-y` to actually rename files.

Remove a long suffix:

```bash
xSHELL ren -x .chr.removedTE.v1.longest.pep.fa *.fa
```

Change extension `fa` to `fasta`:

```bash
xSHELL ren -e fa:fasta *.fa
```

Rename one FASTQ file:

```bash
xSHELL ren -f _1.fastq -t .RNAseq_1.fastq ALB27_1.fastq
```

Batch rename FASTQ files and execute:

```bash
xSHELL ren -f _1.fastq -t .RNAseq_1.fastq *_1.fastq -y
```

Common options:

- `-x TEXT` or `--strip TEXT` removes TEXT when it appears at the end of the filename
- `-e OLD:NEW` or `--ext OLD:NEW` changes extension OLD to NEW
- `-f TEXT` or `--from TEXT` selects text to replace
- `-t TEXT` or `--to TEXT` sets replacement text
- `-y` or `--yes` executes the rename
- `-n` or `--dry-run` previews only, the default

## size

`size` shows disk usage for immediate child directories.

Show child directory sizes under the current directory:

```bash
xSHELL size .
```

Sort largest first:

```bash
xSHELL size -r .
```

## top

`top` shows a process snapshot. It is useful for quickly answering:

- Which jobs is one user running?
- What is one PID doing?
- Which processes use the most CPU?
- Which processes use the most memory?
- Who is running a command such as `java`, `bwa`, or `blast`?

Show current processes, sorted by CPU usage by default:

```bash
xSHELL top
```

Show tasks from one user:

```bash
xSHELL top -u USER
```

Show one PID:

```bash
xSHELL top -p 1270479
```

Sort by memory:

```bash
xSHELL top -s mem
```

Sort by CPU usage and show the first 50 rows:

```bash
xSHELL top -s cpu -n 50
```

Filter processes whose command contains `java`:

```bash
xSHELL top -u USER -g java
```

Refresh every 5 seconds:

```bash
xSHELL top -s mem -w 5
```

Common options:

- `-u USER` or `--user USER` shows processes owned by one user
- `-p PID` or `--pid PID` shows one PID, or comma-separated PIDs such as `PID1,PID2`
- `-g TEXT` or `--grep TEXT` keeps processes whose command contains `TEXT`
- `-s cpu` or `--sort cpu` sorts by CPU usage
- `-s mem` or `--sort mem` sorts by memory percentage
- `-s rss` or `--sort rss` sorts by resident memory size
- `-s time` or `--sort time` sorts by elapsed running time
- `-n N` or `--limit N` shows the first N rows
- `-w SEC` or `--watch SEC` refreshes every SEC seconds

## Suggested Aliases

Add these to `~/.zshrc` or `~/.bashrc`:

```bash
alias xs='xSHELL'
alias refgenome='xSHELL path ref-genome'
```

Then use:

```bash
xs ext -e tsv -m .
xs find '*.fa*' -s size -r .
refgenome
```
