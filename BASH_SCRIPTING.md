# Bash scripting

Contains notes on bash: syntax, constructs, behaviors. Not standalone commands  
you'd run interactively.

**Belongs here**: variables & parameter expansion (`$1`, `$@`, `$#`, `$?`),  
conditionals (`[[ ]]`, `[ ]`), arithmetic, loops, functions, arrays, anything  
about how bash *executes* code.

**Does NOT belong here:** standalone programs/builtins you'd type to do something  
(`grep`, `find`, `cd`, `ls`) - those go into `TERMINAL_COMMANDS.md`.

## Table of Contents

* [Comments](#Comments)
* [Globbing](#Globbing)
* [Variables](#Variables)
* [Special Variables](#Special-variables)
* [Arrays](#Arrays)
* [Expanding](#Expanding)
* [Quoting and word splitting](#Quoting-and-word-splitting)
* [Comparison operators](#Comparison-operators)
* [Test command](#Test-command)
* [Extended test command](#Extended-test-command)
* [Arithmetic evaluation](#Arithmetic-evaluation)
* [Arithmetic expansion](#Arithmetic-expansion)
* [Conditionals - if-elif-else-fi](#Conditionals---if-elif-else-fi)
* [Conditionals - if-elif-else-fi oneliners](#Conditionals---if-elif-else-fi-oneliners)
* [Conditionals - case-esac](#Conditionals---case-esac)
* [Loops - for](#Loops---for)
* [Loops - while](#Loops---while)
* [Exit status](#Exit-status)
* [Get an argument](#Get-an-argument)
* [Check number of arguments](#Check-number-of-arguments)
* [Brace expansion](#Brace-expansion)
* [Command substitution](#Command-substitution)
* [Subshell](#Subshell)
* [Sourcing](#Sourcing)
* [Connectors](#Connectors)
* [Redirection](#Redirection)
* [Functions](#Functions)
* [Heredoc](#Heredoc)

## Comments

Everything after # is ignored. It can be added at the end of a running command.

Prints 1 2 3:
```
echo 1 2 3 # 4 5 6
# this wont do anything
```

## Globbing

Examples based on ls command.  
This happens at the shell parsing stage, not inside the command.  
When `echo *.md` is written, bash finds what matches it and rewrites the command.


Matches any number of characters

```bash
ls *
ls *.md
```

Matches one character

```bash
ls file?.txt
cat ./fan-temp-no-??/temp.log
```

Matches any character from set

```bash
ls file[123].txt
```

Matches any character not from set

```bash
ls file[!123].txt
ls file[^123].txt
```

## Variables
...

## Special variables

```text
IFS - Internal Field Separator. Bash shell variable related to Bash word splitting.
```

## Arrays
...

## Expanding
...

## Quoting and word splitting

Unquoted - full expansion + word-splitting + globbing

```bash
echo a b c  # 3 separate characters.
```

Single quotes - no expansion at all. Preserving literal value of each character.  
$VAR stays as text. Can't put (') inside also with esc character \.

```bash
echo '$VAR'
echo 'echo $(cat a_file.txt) >> b_file.txt' >> some_script.sh  # Puts cmd in script
echo 'a b c'  # One string. No word splitting
```

Double quotes - variables and commands expand. Globbing, word splitting suppressed.

```bash
echo "$VAR"  # Prints value
echo "\""  # Escape characters work - output (")
echo "$(cmd)"  # Expands command
echo "a b c"  # One string. No word splitting
```

Word-splitting

```bash
./print_first_arg.sh a b c  # 'a'
./print_first_arg.sh 'a b' c  # 'a b'
./print_first_arg.sh "a b" c  # 'a b'
```

Using quotes may give different results:

```bash
./script.sh "a b c"  # one argument
./script.sh 1 'a b'  # two arguments
./script.sh $(cat a_file)  # 14 args
./script.sh "$(cat a_file)"  # 5 args
./script.sh '$(cat a file)'  # 1 arg (not expanded)
```

## Comparison operators

Use these in a test command - see test command paragraph.  
Usually we thing about such tests as 'a compared to b', but  
some of these flags just check things on one argument.  
This way it may look like 'check b', e.g.: '-x /path/scr'.

Comparing numbers

```text
-eq - Equal
-ne - Not equal
-lt - Less than
-le - Less than or equal
-gt - Greater than
-ge - Greater than or equal
```

Comparing strings

```text
=   - Equal
==  - Equal
!=  - Not equal
<   - If comes before alphabetically
>   - If comes after alphabetically
```

Files and directories

```text
-e  - exists
-f  - exists and is a regular file
-d  - exists and is a directory
-r  - readable
-w  - writable
-x  - executable
-s  - exists and is not empty
-z  - exists and is empty
-L  - symbolic link
```

## Test command

Use these e.g. in an 'if' statement. Check status of a previously executed  
test with 'echo $?'.

Syntax rule

```text
Put spaces around arguments and operators. It is done so Bash does not treat
two parts as one variable
[$name treated as one
$name=="sth" as one.
```

```bash
# Bracket form
[ "$#" -ne 2 ]

# No comparison, just check
[ -x /usr/bin/sth ]

# Negation in brackets
[ ! "$VAR" -eq 2 ]

# Text form
test "$#" -ne 2

# Negation in text form
test ! "$VAR" -eq 2
```

Connecting tests with logical operators (different than in [[ ]])

```text
# Logical operators inside
-a	- and
-o	- or
[ sth -a sth2 -o sth3 ]

# Logical operators outside
&& || !
[ test1 ] && [ test2 ] || [ test3 ]
```

## Extended test command

It is a test command that has additional abilities in comparison with the basic one.  
This one should be usually used instead of the basic one, but the basic one can be  
used across different shells. This one may not work in other ones.  
The syntax rules are the same as in the basic test. Everything should be separated with spaces.

```bash
# No word splitting by default example with comparison to single-bracket tests
SOME_NAME="First Second"
[ "$SOME_NAME" == "First Second" ]; echo $?   # Quoted variable not splitted - ok
[ $SOME_NAME == "First Second" ]; echo $?     # Unquoted variable splitted - fail
[[ "$SOME_NAME" == "First Second" ]]; echo $?  # Quoted variable not splitted - ok
[[ $SOME_NAME == "First Second" ]]; echo $?    # Unquoted variable NOT splitted - ok
```

```bash
# Pattern matching
[[ $file == *.txt ]]
```

```bash
# Regex =~
```

```bash
# Boolean operators inside the brackets && ||
```

## Arithmetic evaluation

```text
(( ... ))
```

## Arithmetic expansion

```text
$(( ... ))
```

## Conditionals - if-elif-else-fi

if

```bash
if [ "$#" -ne 2 ]; then
	echo "No. of args not equal to 2"
fi
```

if-else

```bash
if [ "$#" -eq 2 ]; then
	echo "No. of args equal to 2"
else
	echo "Not 2 arguments"
fi
```

if-elif-else

```bash
if [ "$#" -eq 1 ]; then
	echo "One arg"
elif [ "$#" -eq 2 ]; then
	echo "Two args"
else
	echo "Other num. of args"
fi
```

nested if
```bash
if [ "$#" -eq 1 ]; then
	if [ "$2" -ne "off" ]; then
		set_up_something.sh
	fi
fi
```

if with command output - passes the command printed output to if

```bash
if [ "$(./get_mount_name.sh 2>/dev/null)" == "/some/mount/name" ]; then
	echo "Mounted (based on the passed string check)"
fi
```

if with exit code - passes the command exit code to if

```bash
if grep -q "text" a_file.txt; then
	echo "It's there"
fi

if ./check_mount.sh; then
	echo "Mounted (based on exit code check)"
fi
```

## Conditionals - if-elif-else-fi oneliners

Due to hard readability it is not advised to make conditionals larger than  
a simple 'if' as oneliners.

if

```bash
if [ "$#" -eq 2 ]; then echo "2 args passed"; fi
```

if-else

```bash
if [ "$#" -eq 2 ]; then echo "2 args"; else echo "Other num. of args"; fi
```

if-elif-else

```bash
if [ "$#" -eq 1 ]; then echo "1 arg"; elif [ "$#" -eq 2 ]; then echo "2 args"; else echo "Other num. of args"; fi
```

## Conditionals - case-esac

Basic syntax  
The case statement supports standard Bash globbing patterns (*, ?, [], [^])

```bash
case $VAR in
	pattern1)
		# code to run if variable matches pattern1
		;;
	pattern2)
		# code to run if variable matches pattern2
	*)
		# default code to run if no patterns match (optional)
		;;
esac
```

OR logic using single pipe

```bash
case $CHOICE in
	[yY] | [yY][eE][sS])
		echo "You said yes"
		;;
	[nN] | [nN][oO])
		echo "You said no"
		;;
	*)
		echo "Invalid response"
esac
```

AND logic using concatenation ':'

```bash
case $ROLE:$STATUS in
	"admin:active")
		echo "Access granted"
		;;
	"user:active")
		echo "Limited access"
		;;
	*:inactive)
		echo "Account inactive"
	*)
		echo "Unknown combination"
esac
```

Checking multiple matches using ;;& - may not work in older versions of Bash

```bash
case $NUMBER in
	*[05])
		echo "The number is divisible by 5"
		;;&  # Continue checking further matches despite a match
	*[13579])
		echo "The number is odd"
		;;
esac
```

Blind fall-through - execute the next block without pattern matching  
Admin will print 3 rows, user 2 and guest 1.

```bash
case $ROLE in
	"admin")
		echo "Delete and modify files"
		;&
	"user")
		echo "Create and edit files"
		;&
	"guest")
		echo "View files"
		;;
esac
```

## Loops - for

```bash
# For loop - oneliner. Uses {1..10} which avoids using external seq cmd.
for i in {1..10}; do some_steps.sh; done

# Using a limited number of arguments
for i in arg1 arg2 arg3
do
	some_steps.sh
done

# Using seq (and command expand in general)
for i in $(seq 1 10)
do
	some_steps.sh
done
```

## Loops - while

```bash
# until loop difference
# break and continue
# nested loops
```

## Exit status

Every command and script in Bash returns an exit status when it finishes.
Exit status is checked in the conditionals to determine the true/false values.

```text
0 - success
non-zero - failure or another non-success condition
```

The exit status of a previously executed command can be checked with

```bash
echo $?
```

The status in a script can be set with the following command

```bash
# Exit without failure
exit 0

# Indicate a failure exit e.g. in an if-else conditional
# Various values can mean different issues to provide useful info about the fail status.
exit 1   # May indicate general, not catched fail
exit 13  # May indicate some parsing error
```

## Get an argument

These are special variables used to retrieve arguments passed to the  
script in which they are called.

```bash
echo "Number of arguments: $#"
echo "All arguments listed: $@"
echo "Script name: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "Third arg: $3"
```

## Check number of arguments

Example for 3 args.

```bash
# Variable
# Because it's a single number, there is no theoretical reason for quotes,
# but it follows Bash convention:
# Quote variables unless you specifically need shell expansion behavior.
echo $#
echo "$#"

# Practical usage example
if [ "$#" -ne 3 ]; then
	# Do the operations

	# Script can be stopped with:
	exit 1
fi
```

## Brace expansion

Provides various ways to expand a range into values.
Don't use spaces inside the brackets, or it will not work correctly!

```bash
# echo {n..k} includes both <n:k> with step 1
echo {1..10}

# echo {n..k} where n > k
echo {10..1}

# echo {n..k..s} includes both <n:k> with step s
echo {1..10..2}

# echo {n..k..s} where n > k
echo {10..1..2}

# echo {n..k} where the range is in characters
echo {a..f}

# echo range of characters with a step
echo {a..h..2}

# expand predefined values
# NOTE: No spaces between characters!
# Spaces will determine delimiters between files names.
touch file_{100,200,300}

# nesting expansion
echo a{s{1,2,3},g{1,2,3}}  # out: as1 as2 as3 ag1 ag2 ag3

# cartesian combination
echo s{1,2}{3,4}  # out: s13 s14 s23 s24

# usage examples
mkdir dir{1..10}
echo dir{1..10}
```

## Command substitution

Run a command and substitute its output into the surrounding command line as text.  
It is important that this will pass the text, not the exit code e.g. to if statement.

```bash
# Print output from a command/pipe/etc.
echo $(ls *.txt | grep Sample_*)
SOME_VAR=$(python3 -c "print('ABCD1234')")
```

## Subshell

Run a command in an isolated child process. No output capture.
Use e.g. to run a 'cd' that you dont' want to leak into the current shell.

```
# Usage
(action_inside)			- Syntax
(cd /)				- Will go to the root dir in the subshell but won't move in the current one.

# Here due to the syntax it will be interpreted as array, not subshell
VAR=(date)
echo $VAR  # '(date)'

# To properly assign a previous command success status use this instead
(date); VAR=$?

# Multiline subshell
(
	cd ~/
	mkdir temp1
	./do_something.sh
	rmdir temp1
)
```

## Sourcing

```bash
source script.sh
. script.sh
```

## Connectors

```text
|
&&
||
;
```

## Redirection

```text
>
>>
>>>
<
2>&1
/dev/null
```

## Functions

Basic syntax

```bash
# Body
my_function() {
	local VARIABLE=3
	echo "func_body"
}

# Call
my_function
```

Passing arguments is like passing them to a script

```bash
# Body
my_function() {
	echo "Number of arguments passed: $#"
	echo "Passed arguments: $@"
}

# Calls
my_function
my_function one_arg
my_function 1 2
my_function 1 '2 a'
my_function 1 '2 a' $VAR
my_function file_{1..20}.temp
```

## Heredoc

```bash
cat<<NAME
[...]
NAME
```

```bash
cat<<'NAME'
[...]
NAME
```

## Scripting - shebang

...

## Scripting - execution permission

...
