## usage

```text
git <command> <flags>					- The following commands are the 'git' command options.
```

## branch

	-M <name>					- Forcefully rename the current branch to that name.
	-r						- List remote branches for the repository.

## cherry-pick

	commit-hash					- Apply a specific commit on top of the current branch.

## clean

	-f						- Remove the files listed with '-n'. By default git won't remove files
							  without this flag.

	-n						- Dry-run what would be removed with git clean on a repository.
							  Remove unwanted files from repo, e.g. if scripts create a lot of stuff
							  that is not gitignored and populates the project directory.

## config

	--local/--global
		user.name "Your name here"		- Change name at the specified level.
		user.email "your@email.example"		- Change mail at the specified level.
	list/--list					- List config. Can mix with '--local'.
		--local					- Show local configuration.

## commit

```text
--amend						- Amends previous commit message.
--amend --author "YourName <name@mail.com>" 	- Amends the credentials for a previous commit.
```

## fetch

```text
	--prune						- Remove local branches that don't have matching remote ones.
```

## init

```text
()						- ...
--bare						- No working tree (requires further explanation).
```

## log

```text
--all						- Show also commits from other branches. Used with --graph to visually represent branches.
--graph						- Show in a form of graph. Used with --all to show commits from other branches.
--oneline					- Show log one line per commit.
-n N						- Show N commits counting from the top.
-N						- Show N commits counting from the top.

--oneline --graph --all -n 10			- Combining flags.
```

## ls-remote

	()						- List references in remote repo.
							  Fails if no connection.

## merge

	--ff-only <branch-name>				- Merge only if fast-forward possible. Do while on target branch.

## pull

	remote_alias branch_name			- Pull the branch specified with branch_name from alias name repo.

## push

```text
remote_alias --delete branch_name		- Remove a remote branch. Does not remove its matching local branch. For that read about fetch --prune.
						  Example: git push origin --delete fix_branch
-u/--set-upstream origin <name>			- Link local branch to a remote branch.
```

## rebase

	-i						- Interactive rebase.

## remote

```text
add alias /some/other/local/path/		- Specify the remote repo with a local path. Used to configure a local remote repository.
add alias user@ip:/path/to/repo			- Specify a remote repo for a local one. It will be configured for push/pull (see with remote -v)
remove alias					- Remove a configured remote repo for a local one.
-v						- Show remote links for pull and push.
get-url origin					- Get the remote repo link.
set-url origin <ssh repo link>			- Used to change HTTPS link to SSH one.
set-url --push alias DISABLED			- Disable push for the remote repo specified with the alias name.
```

## reset

```text
--soft HEAD~1					- Uncommit the last commit and leave the changes staged. Undo the commit but have the things ready.
--mixed HEAD~1 (also default w/out flag)	- Uncommit the last commit but just leave the changes as usual files modifications.
--hard HEAD~1					- Uncommit the last commit and throw away the changes.
```

## switch

	-c feature_br_name --track alias/feature_br_name - Create a local branch for a remote one and immediatelly pull all the content to it so they match.
	--track alias/feature_br_name			- Shorter version of the -c feature track local branch creation.
