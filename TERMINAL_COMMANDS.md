# Terminal commands

This document lists self-sufficient commands.
Some of them may require additional instalation. This information is noted in their description.

() - means the command with args from a tree. Used to minimize repetitions.  
E.g.: cmd -> arg -> subarg -> () --flag - Here () means 'cmd arg subarg'  
and the whole command is: 'cmd arg subarg --flag'

## Table of Contents

* [Commands](#Commands)
	* [cd](#cd)
	* [date](#date)
	* [echo](#echo)
	* [env](#env)
	* [ffmpeg](#ffmpeg)
	* [less](#less)
	* [ls](#ls)
	* [mkdir](#mkdir)
	* [mktemp](#mktemp)
	* [more](#more)
	* [pgrep](#pgrep)
	* [ping](#ping)
	* [pkill](#pkill)
	* [printenv](#printenv)
	* [pwd](#pwd)
	* [qpdf](#qpdf)
	* [rsync](#rsync)
	* [set](#set)
	* [seq](#seq)
	* [tar](#tar)
	* [tcpdump](#tcpdump)
	* [tee](#tee)
	* [tree](#tree)
	* [wc](#wc)
* [Commands grouped by functions](#commands-grouped-by-functions)

## Commands

### cd

```bash
# Go to a directory
cd /some/directory/path

# Go one directory above
cd ..

# Go to home directory
cd
cd ~

# Go to root directory

cd /

# Go back to the previous directory, e.g. after you did `cd ~`.
cd -
```

### date

```bash
# Date in a format useful for appending to a log name
NOW_DATE=$(date '+%Y%m%d-%H%M%S')
```

### echo

```text
echo <text>			- simly print the text
	() -n <text>		- don't add newline at the end
```

### env

```bash
# Print names and values of variables in the environment
# For this task, both 'env' and 'printenv' behave identically.
env
```

### ffmpeg

```bash
# Basic compression of mp4 file to make it smaller (crf 18 Very-HQ, 23 default, 28 smaller file)
ffmpeg -i input.mp4 -vcodec libx264 -crf 28 output.mp4

# Smaller file. More aggressive compression (-preset slow - better compression, slower encoding)
ffmpeg -i input.mp4 -vcodec libx264 -crf 32 -preset slow output.mp4

# Change resolution - scales to 1280px width and keeps the aspect ratio
ffmpeg -i input.mp4 -vf scale=1280:-1 -crf 28 output.mp4

# Limit bitrate
ffmpeg -i input.mp4 -b:v 1000k -b:a 128k output.mp4

# Cut first N (10) seconds (-c copy. No re-encoding, no quality loss. May not be precise):
ffmpeg -ss 10 -i input.mp4 -c copy output.mp4

# Precise cut (reencoding)
ffmpeg -i input.mp4 -ss 10 -c:v libx264 -crf 23 -c:a aac output.mp4

# Cut + compression
ffmpeg -ss 10 -i input.mp4 -vcoded libx264 -crf 28 output.mp4

# Cut a segment
ffmpeg -ss 10 -t 30 -i in.mp4 -c copy out.mp4
```

### less

```text
less -R	- Print text with ASCII control characters interpreted (e.g. color).
```

### ls

Flags

```
-1
-a
-h
-l
-F		- Adds additional character to each name that indicates the type, e.g. directory/ script.sh*
-L
```

Usage - flags can be connected

```
ls -alhF
```

It is common to add aliases for various ls modes in the configuration file - here ~/.bashrc
Example aliases

```
alias ll='ls -alh'
alias lla='la -ls -A'
```

### mkdir

```
mkdir
mkdir -p
```

### mktemp

```bash
# Create a temporary file in /tmp/
mktemp

# Create a temporary directory in /tmp/
mktemp -d
```

### more
Older version of 'less'. No option for going back. Use 'less' instead.

### pgrep

Return the PID of a process requested by name.

```
# Simple request that will search for a partial match: process_name1 and process_name2 are a match
pgrep process_name

# Searching for an exact match
pgrep -x process_name

# Kill process by name. It will return an error if proc not found. Use pkill instead.
kill -9 $(pgrep -x process_name)
```

### ping

Ping command is available on different OS's, but its flags will vary in behavior.

### pkill

```
# Kill a process by name
pkill -9 -x process_name
```

### printenv

Print all Linux variables used in the OS.
For this task, both 'env' and 'printenv' behave identically.

```text
Example output:

USER=user
COLORFGBG=10;0
SHLVL=2
XDG_SESSION_ID=3
...
```
### pwd

Show the current location path. Can be used to confirm a location or put the output  
into a script.

```bash
# Output e.g.: /home/user/program/subdir/
pwd
```

### qpdf

PDF file manipulation. Requires installation.

```bash
# Page count
qpdf --show-npages file.pdf

# Full metadata + structure check
qpdf --check file.pdf

# Show file info
qpdf --show-encryption file.pdf

# Extract pages
qpdf input.pdf --pages input.pdf 3-5  # range
qpdf input.pdf --pages input.pdf 3 5 10  # specific
qpdf input.pdf --pages input.pdf 5 1 9 3  # reorder

# Merge/split
qpdf --empty --pages a.pdf b.pdf c.pdf -- output.pdf
qpdf --linearize input.pdf output.pdf  # requires additional anlaysis

# Encryption/decryption
qpdf --password=PASS --decrypt input.pdf output.df
qpdf --encrypt userpass ownerpass 256 --input.pdf output.pdf  # user, owner, key-length
qpdf --stream-data=compress input.pdf output.pdf
qpdf --object-streams=generate --compress-streams=y input.pdf output.pdf

# Page manipulation
qpdf input.pdf --rotate=+90:2-5 -- output.pdf
```

### rsync

Synchronize an external directory to a specified target one. Good for backup.  
Faster than scp because it can ommit files that are already there.

```text
# Flags
() --delete	- this flag ensures that it is the exact copy. Removes things also from backup.
() -a		- preserve permissions, timestamps, etc.
() -v		- verbose
() -P		- progress + resume support

# Example execution:
rsync -avP --dry-run --exclude=".ssh" user@192.168.10.10:~/ /home/local-user/backup/
```

### set

```
set -euo pipefail
```

### seq

Flags

```text
-s "<sep>" / --separator=<sep>			- Define a separator. Default is '\n'.
-w/--equal-width				- Fill the numbers with leading zeros. Takes no arg.
-f "<format>" /--format=<format>		- Define the format in which numbers will be presented.
						  Includes: "%a", "%e", "%f", "%g", "%A", "%E", "%F", "%G"
```

Example usage

```bash
# inclusive sequence: <1:num>
seq 100

# inclusive sequence: <num1:num2>
seq 5 10

# inclusive sequence with step: <num1:step:num2>
seq 1.5 -0.5 -15.3
```

### tar

Flags

```text
-x				- Unpack files from archive.
-z				- Unpack gzip compression.
-f				- Specfiy filename.
```

Usage example
```
tar -xzf file.tar.gz		- Unpack tar.
```

### tcpdump

```
sudo tcpdump -i eth0		- Listen for data on eth0 Ethernet network interface.
```

### tee

Redirect the output to both stdout and a log file.

```bash
./setup-configuration.sh | tee configuration.log
```

### tree

```text
()			- Show the directory tree starting from the current one.
() -L <n>		- Specify how deep should the tree go.
() ./start/dir		- Show the directory tree starting from the specified one.
```

### wc

Word-count

Flags

```text
-c <file>		- Characters
-w <file>		- Words
-l <file>		- Lines
```

Usage

```bash
# Check number of lines in a file a_file.txt
cat a_file.txt | wc -l
```

## Commands grouped by functions
| Category | Description | Example commands |
|---|---|---|
| Navigation and inspection | Where am I, what's here, where is X | `cd`, `ls`, `tree`, `pwd`, `find`, `which` |
| Viewing file contents | Reading text-based contenet without editing | `cat`, `less`, `more`, `head`, `tail` |
| Creating, modifying, removing | Changing what exists on disk | `touch`, `mkdir`, `cp`, `mv`, `rm`, `ln`, `mktemp` |
| Searching and text processing | Finding or transforming text content | `grep`, `sed`, `awk`, `sort`, `cut` |
| Permissions and ownership | Who can access/change what | `chmod`, `chown` |
| Compression and archiving | Turning one file type into another | `tar`, `zip`, `gzip`, `7z` |
| Format conversion | Turning one file type into another | `ffmpeg`, `img2pdf` |
| Process and system monitoring | Watching/controlling running processes | `ps`, `pregp`, `top`, `kill`, `htop` |
| Networking | ... | `ping`, `tcpdump` |
| Behavior modifiers | ... | `shopt`, `set`, `export`, `alias`, `unalias` |
