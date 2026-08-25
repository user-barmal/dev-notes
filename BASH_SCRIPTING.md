# Bash scripting

Contains notes on bash: syntax, constructs, behaviors. Not standalone commands  
you'd run interactively.

**Belongs here**: variables & parameter expansion (`$1`, `$@`, `$#`, `$?`),  
conditionals (`[[ ]]`, `[ ]`), arithmetic, loops, functions, arrays, anything  
about how bash *executes* code.

**Does NOT belong here:** standalone programs/builtins you'd type to do something  
(`grep`, `find`, `cd`, `ls`) - those go into `TERMINAL_COMMANDS.md`.

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

## Quoting, word-splitting

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

## Conditionals

if-elif-else-fi

```bash
# if
if [ "$#" -ne 2 ]; then
	echo "No. of args not equal to 2"
fi

# if-else
if [ "$#" -eq 2 ]; then
	echo "No. of args equal to 2"
else
	echo "Not 2 arguments"
fi

# if-elif-else
if [ "$#" -eq 1 ]; then
	echo "One arg"
elif [ "$#" -eq 2 ]; then
	echo "Two args"
else
	echo "Other num. of args"
fi

# nested if
if [ "$#" -eq 1 ]; then
	if [ "$2" -ne "off" ]; then
		set_up_something.sh
	fi
fi

# if with exit code
# In this case we don't use the $() which would pass the command output as text.
if grep -q "text" a_file.txt; then
	echo "It's there"
fi
```

case-esac

```bash
...
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

## Loops

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
(action_inside)			- Syntax
(cd /)				- Will go to the root dir in the subshell but won't move in the current one.
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
