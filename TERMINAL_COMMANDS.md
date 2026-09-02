# Terminal commands

This document lists self-sufficient commands.
Some of them may require additional instalation. This information is noted in their description.

() - means the command with args from a tree. Used to minimize repetitions.  
E.g.: cmd -> arg -> subarg -> () --flag - Here () means 'cmd arg subarg'  
and the whole command is: 'cmd arg subarg --flag'

## Tables of Contents

* Table of content:
	* [Commands grouped alphabetically](#Commands-grouped-alphabetically)
	* [Commands grouped by functions](#Commands-grouped-by-functions)
	* [Commands](#Commands)
		* commands documentation

## Commands grouped alphabetically

| Char | Cmds |
|---|---|
| a | [apt](#apt), [awk](#awk) |
| b | |
| c | [cd](#cd) |
| d | [date](#date), [docker](#docker), [dpkg](#dpkg) |
| e | [echo](#echo), [env](#env), [exit](#exit) |
| f | [ffmpeg](#ffmpeg), [find](#find) |
| g | [grep](#grep) |
| h | [head](#head) |
| i | [ip](#ip) |
| j | |
| k | |
| l | [less](#less), [ls](#ls) |
| m | [mkdir](#mkdir), [mktemp](#mktemp), [more](#more) |
| n | |
| o | |
| p | [pgrep](#pgrep), [ping](#ping), [pkill](#pkill), [printenv](#printenv), [pwd](#pwd) |
| q | [qpdf](#qpdf) |
| r | [rm](#rm), [rsync](#rsync) |
| s | [sed](#sed), [seq](#seq), [set](#set), [systemctl](#systemctl) |
| t | [tail](#tail), [tar](#tar), [tcpdump](#tcpdump), [tee](#tee), [touch](#touch), [tree](#tree) |
| u | |
| v | [veracrypt](#veracrypt) |
| w | [wc](#wc), [which](#which) |
| x | |
| y | |
| z | |

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
| Networking | ... | `ip`, `ping`, `tcpdump` |
| Behavior modifiers | ... | `shopt`, `set`, `export`, `alias`, `unalias` |

## Commands

### apt

Higher-level package management tool. It can download packages from configured repositories.  
Unlike 'dpkg' it resolves and installs dependencies and can install local '.deb' files.

```
# Install from an external source
sudo apt install screen

# Install a local .deb package
sudo apt install ./package.deb
```

### awk

```
# Print only the last value in a line
awk '{print $NF}' filename.txt
echo "awk will only print: THIS" | awk '{print $NF}'
```

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

### docker

Docker is a large tool. Here are some quick commands used for simple actions.  
Advanced docker tasks won't be discussed here.

Docker image - static, read-only blueprint stored on disk  
Docker container - a live, running instance of that image executing in memory

Flags

```text
exec
	-i		- Interactive. Keeps input connected.
	-t		- Gives a terminal (TTY).
```

```bash
# Open a live, interactive terminal inside a running container
docker exec -it container_name bash

# Copy a file
docker cp container_name:/file/path/within/container /host/path/target

# Create a container from an image
docker run

# List all locally stored docker images
docker images

# List running containers
docker ps

# List running and exited containers
docker ps -a

# Stop a running container gracefully
docker stop

# Stop/kill a running container immediatelly
docker kill

# Delete a stopped container
docker rm
```

### dpkg

Install from a '.deb' package.  
It installs the package, but does not resolve/download dependencies.  
For easy '.deb' installation with dependency handling, use 'apt' instead.

```
# List all packages and search examples
dpkg -l
dpkg -l > packages.log
dpkg -l | grep some_package_name

# Install
sudo dpkg -i PACKAGE_NAME.deb

# Remove
sudo dpkg -r installed-package-name

# Reconfigure
sudo dpkg-reconfigure PACKAGE_NAME.deb
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

### exit

Exits shell  
Many tools also use this keyword to exit, e.g. 'ssh'.

```
exit
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

### find 

...

### grep

grep is a tool for searching for a matching string.  
grep - Name comes from an old syntax g/re/p - global/regex/print.  
It supports basic regex by default and extended regex with -E flag.  
The regex characters are additionally explained in details in the regex file  
dedicated to explain it in various tools as the characters can differ.

```text
# Basic regex characters overview - more escaping
a|b a* a\+ a\? a\{n\} a\{n,k\} \(abc\) [abc] ^line line$

# Extended regex characters overview - cleaner syntax. Use for more complicated patterns.
a|b a* a+ a? {n} a{n} a{n,k} (abc) [abc] ^line line$
```

```bash
# Simple usage - search for a string
grep string filename

# Search with basic regex OR
grep 'case1|case2|case3'

# Search for a line start with 0: or 6: with extended regex:
grep -E '^(0|6):' file
```

### head

...

### ip

Flags

```text
# Pre-flags - ip <pre> option <subopt>
-br				- brief info
-d/--details			- show detailed info
-o/--oneline			- show data one line per object

# The suboption is assumed to be 'show' if not defined. If want to use 'show' it can be ommited.

a/addr/address			- Can anything between the shortest and the longest form.
l/link
	show
		dev <devname>	- Show data for an interface specified by name.
n/neighbor
r/route
	get <ip>		- Show a route from routing table to the specified IP.
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
-A
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
alias lla='ls -lA'
```

### mkdir

```
# Create a directory - will fail if creating in nested directories that don't exist
mkdir /home/user/dir1

# Create nested directories if they don't exist - creates sub3 but also sub2 and sub1 if they aren't there
mkdir -p /home/user/dir1/sub1/sub2/sub3
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

```bash
# Simple ping
ping 192.168.15.15

# Ping from specified interface <interface> <target>
ping -I Ethernet112 10.0.0.57

# Ping from specified IP <source> <target>
ping -I 10.0.0.55 10.0.0.57
```

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

### rm

...

### rsync

Synchronize an external directory to a specified target one. Good for backup.  
Faster than scp because it can ommit files that are already there.

```text
# Flags
() --delete	- this flag ensures that it is the exact copy. Removes things also from backup.
() --exclude	- exclude the provided path from the rsync operation
() -a		- preserve permissions, timestamps, etc.
() -v		- verbose
() -P		- progress + resume support

# Example execution:
rsync -avP --dry-run --exclude=".ssh" user@192.168.10.10:~/ /home/local-user/backup/
```

### sed

...

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

### systemctl

...

### tail

...

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

Analyze the TCP traffic on specified interfaces.

Flags

```text
-e			- Show Ethernet frame header
-i			- Specify interface
-n			- Don't resolve IP addresses to host names
-nn			- Additional to 'n'. Don't resolve port numbers to service names
```

Usage

```bash
# Cmd: sudo tcpdump -i <interface> ... -i <interfaceN> 'prot1 or prot2 ...'

# Listen for data on eth0 Ethernet network interface
sudo tcpdump -i eth0

# Listen for data on eth0 Ethernet network interface. Filter only defined protocol, e.g. arp
sudo tcpdump -i eth0 protocol

# Listen for data on a few interfaces.
sudo tcpdump -i eth0 -i eth1 -i eth2

# Listen for data on all interfaces
sudo tcpdump -i any

# Listen for data on eth0 Ethernet network interface. Filter for specified protocols, e.g. 'arp and icmp'
sudo tcpdump -i eth0 'prot1 or prot2'

# Don't resolve IP addresses to host names (-n)
sudo tcpdump -i eth0 -n prot1

# Additionally don't resolve port numbers to service names (-nn)
sudo tcpdump -i eth0 -nn prot1
```

### tee

Redirect the output to both stdout and a log file.

```bash
./setup-configuration.sh | tee configuration.log
```

### touch

...

### tree

```text
()			- Show the directory tree starting from the current one.
() -L <n>		- Specify how deep should the tree go.
() ./start/dir		- Show the directory tree starting from the specified one.
```

### veracrypt

Official documentation: [Veracrypt command line usage for Linux and macOS](https://veracrypt.io/en/Command%20Line%20Usage%20for%20Unix.html)

Requires installation from a .deb package.

```
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

### which

...

