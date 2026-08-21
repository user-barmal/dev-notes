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
