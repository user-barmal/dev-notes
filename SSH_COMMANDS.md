## scp

...

## ssh

```
ssh -q					- Runs ssh in quiet mode without any warning or welcomming
					  messages. Used especially for scripting.
ssh ${MY_USER}@${SERV_IP_ADDRESS}	- ssh with credentials passed in variables.
ssh <name>				- ssh with Host name defined in ~/.ssh/config (see below)
ssh -o <option>=<value>			- Start ssh with additional options.
```

SSH options and their meaning

```text
ServerAliveInterval <s>			- ...
ServerAliveCountMax <no>		- ...
```

## ssh-keygen

```text
ssh -R <IP-address>		- remove an entry in ~/.ssh/known_hosts for this IP-address.

ssh -y -f ~/.ssh/id_rsa		- derive the public key from private one.
```

## ~/.ssh/config

```
# Apply options to all hosts
Host *
	Option_1 val_1
	Option_2 val_2

# Host definition
Host <name>
	HostName <real_host_ip>
	User <username>
	ServerAliveInterval <seconds>
	ServerAliveCountMax <no>
```
