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

## Check number of arguments

Example for 3 args.

```bash
#!/bin/bash
if [ "$#" -ne 3 ]; then
	# Do the operations

	# Script can be stopped with:
	exit 1
fi
```

## Get an argument

Get the arguments:

```bash
echo "Number of arguments: $#"
echo "All arguments listed: $@"
echo "Script name: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "Third arg: $3"
```

Using quotes may give different results:

```bash
./script.sh "a b c"  # one argument
./script.sh 1 'a b'  # two arguments
./script.sh $(cat a_file)  # 14 args
./script.sh "$(cat a_file)"  # 5 args
./script.sh '$(cat a file)'  # 1 arg (not expanded)
```

## Expand range

Expand a range of values with default 1 step, or with every n-th value:

```bash
# echo {n..k} includes both <n:k> with step 1
echo {1..10}

# echo {n..k..s} includes both <n:k> with step s
echo {1..10..2}

# special usage examples (w/out space)
mkdir dir{1..10}
echo dir{1..10}
```

## Subshells

```bash
# Print output from a command/pipe/etc.
echo $(ls *.txt | grep Sample_*)
SOME_VAR=$(python3 -c "print('ABCD1234')")
```
