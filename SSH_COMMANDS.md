# SSH on a Linux system

SSH lives in a few files, tools and directories on a typical system.  
This document is created to discuss the possibilities of it all.

```text
SSH tools can be divided on:

Client programs

	ls -1 /usr/bin/ssh*

	ssh
	ssh-add
	ssh-agent
	ssh-argv0
	ssh-askpass
	ssh-copy-id
	ssh-import-id
	ssh-import-id-gh
	ssh-import-id-lp
	ssh-keygen
	ssh-keyscan

	and additionally
	scp
	sftp

User's SSH directory

	config
	known_hosts
	authorized_keys
	<key files>

System-wide configuration

	/etc/ssh/
	ssh_config
	sshd_config
	<host keys>

The SSH server

	/usr/bin/sshd
	systemctl status ssh
	systemctl status sshd
```


## scp

...

## ssh

### Host authentication vs. User authentication

When you log in for the first time, the prompt you see is for Host Authentication.  
Remote server shows you its Server Public Key (host key).  
The purpose is to prove the server is who it claims to be. Protects from "man-in-the-middle" attack.  
To be secure, you should have an independent, trusted source to verify that the fingerprint  
(the short text summary of the server's public key) shown on your terminal matches the actual server.  
On a large public platform, when you first ssh, your terminal shows a fingerprint.  
You manually check it against the verified hashes listed on the official documentation.  
If they match, then you can safely click 'yes'.  
When you type 'yes', that public key is saved int your local ~/.ssh/known_hosts file.

When you connect to an SSH server, User Authentication is the process where the server checks  
your identity to decide if you are allowed to log in. This happens after you have verified the  
server's identity. Two methods for that include: password or a public key authentication.
With password you write it when prompted, and based on that server gives you the access.  
With key pairs, you keep the private key on your local computer.  
You copy the Public Key to the remote server ahead of time, placing it in a specific file  
~/.ssh/authorized_keys under your user account.
When you try to connect, server sends a challenge encrypted with your public key.  
Only your private key can decrypt it. The solution is sent back to server.  
If correct, you are granted the access.

Example:
I had a case where a device was configured to not allow password based connection.  
It had a pair of keys configured in a way that if you had the key, you were able to log in  
by specifying the key in the ssh command. The shared key was a private key, and on the device  
was a matching public key. The device was the server side with the user public key stored in  
the /root/.ssh/authorized_keys.

To log in used:

```bash
ssh -i root_key root@ip_address
```

### Commands

```bash
# Run in quiet mode w/out any warning or welcomming messages. Used especially for scripting.
ssh -q

# SSH with credentials passed in variables
ssh ${MY_USER}@${SERV_IP_ADDRESS}

# SSH with host name defined in ~/.ssh/config (for the configuration see below)
ssh name

# SSH with additional options
ssh -o option_name=value

# SSH with your private key specified with a flag
# use e.g. if you manage multiple keys for multiple connections
ssh -i /path/to/private_key username@remote_host
```

SSH options and their meaning

```text
ServerAliveInterval <s>			- ...
ServerAliveCountMax <no>		- ...
IdentityFile ~/.ssh/id_ed25519		- Use this identity as one of the identities available for authentication.
					  Depending on the configuration, ssh may or may not try other keys after that.
					  Useful if site (e.g. github.com) tries to use wrong key and fails
					  and this is the one you specified on the site as the authentication one.
IdentitiesOnly yes			- Only use the identities explicitly configured for this connection
```

## ssh-keygen

```text
ssh -R <IP-address>		- remove an entry in ~/.ssh/known_hosts for this IP-address.

ssh -y -f ~/.ssh/id_rsa		- derive the public key from private one.

# Create a pair of ed25519 keys
ssh-keygen -t ed25519

# Print the fingerprint of an SSH key
ssh-keygen -lf ~/.ssh/id_ed25519.pub
```

## /etc/ssh/sshd_config

```text
PasswordAuthentication no
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
	IdentityFile ~/.ssh/id_rsa
	IdentitiesOnly yes
```
