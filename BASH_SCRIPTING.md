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
```bash
echo "Script name: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "Third arg: $3"
```
