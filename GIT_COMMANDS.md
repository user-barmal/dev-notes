## branch

	-M <name>					- Forcefully rename the current branch to that name.

## config
	--local/--global
		user.name "Your name here"		- Change name at the specified level.
		user.email "your@email.example"		- Change mail at the specified level.
	list/--list					- List config. Can mix with '--local'.
		--local					- Show local configuration.

## commit

	--amend						- Amends previous commit message.
	--amend --author "YourName <name@mail.com>" 	- Amends the credentials for a previous commit.

## ls-remote

	()						- List references in remote repo.
							  Fails if no connection.

## push

	-u/--set-upstream origin <name>			- Link local branch to a remote branch.

## remote

	-v						- Show remote links for pull and push.
	get-url origin					- Get the remote repo link.
	set-url origin <ssh repo link>			- Used to change HTTPS link to SSH one.
