branch

	-M <name>					- Forcefully rename the current branch to that name.

config

	--list						- List config. Can mix with '--local'.
	--local						- Show local configuration.

commit

	--amend						- Amends previous commit message.
	--amend --author "YourName <name@mail.com>" 	- Amends the credentials for a previous commit.

push

	-u/--set-upstream origin <name>			- Link local branch to a remote branch.

remote

	-v						- Show remote links for pull and push.
	set-url origin <ssh repo link>			- Used to change HTTPS link to SSH one.
