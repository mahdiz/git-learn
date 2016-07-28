# Learning Git

#### HEAD
The current commit your repository is on. Most of the time HEAD points to the latest commit in your branch.

#### master
The name of the default branch that git creates for you when first creating a repo. In most cases, "master" means "the main branch".

#### origin
The default name that git gives to your main remote repository.

## Creating a Repository
```bash
git init
git add .
git commit -m "First commit"
git remote add origin <URL>
git push origin master
```

## Adding/Removing Files for Commit

#### See local changes
```bash
git status
```

#### Add files to the next commit (aka, stage for commit)
```bash
git add .                      # Add all modified files (tracked and untracked) in the current directory
git add -u .                   # Add only modified tracked files in the current directory
git add file1.txt file2.txt    # Add specific files
```

#### Cancel add (aka, ustage for commit)
```bash
git reset				                   # Undo "git add ."
git reset file1.txt, file2.txt     # Undo specific files
```

#### Untrack a file (never check for changes)
By default, git tracks all files not listed in `.gitignore`. To untrack a file:
```bash
git update-index --assume-unchanged file.txt
```
Files untracked using this command will not be shown under "Untracked files" in `git status`.

#### Track a file
```bash
git update-index --no-assume-unchanged file.txt
```

## Commits

#### Commit changes
```bash
git commit -m "new changes message"
```

#### Send commits to repository
```bash
git push origin master
```

#### Amend a commit
When forgotten something in a commit and want to add it later.
```bash
git add forgotten_file
git commit --amend
```

#### Show commit history
```bash
git log                            # Detailed
git log --pretty=short             # Short
git log --pretty=oneline           # Each commit in one line
git log --author="Mahdi Zamani"    # Commits made by a person
git log --grep=word                # Search in commits messages
```

#### Show files changed in a commit
```bash
git diff-tree --name-only -r ffd96afe9f9d33abea3ebbbfd15b47ea8bd7c2a9
```

## Updating

#### Update local repository with remote changes
```bash
git pull       # Does a git fetch followed by a git merge
```

#### Update local with remote commits (only refs)
This is used to see what everybody else has been working on without actually getting the changes
```bash
git fetch	     # Never changes any of local changes
```

#### Get a local copy of a remote repo
```bash
git clone https:#github.com/moitias/delve.git
```

#### Clone a specific commit
```bash
git init
git remote add origin url://to/source/repository
git fetch origin <sha1-of-commit-of-interest>
git reset --hard FETCH_HEAD			# reset this repository's master branch to the commit of interest (if needed)
```

## Reverting Changes

#### Revert the commit we just created
```bash
git revert HEAD
```

#### Undo (revert) all local changes
```bash
git reset --hard	# Undos changes in tracked files
git clean -df		# Removes untracked files (recommended after "git reset --hard")
git checkout -- <file_name>   # Replace with remote version
```

## Merging

To merge remote changes with local changes:
```bash
git fetch
git commit -m "Local changes"
git pull     # Merges all remote changes with local changes and creates a new commit with message "Merge branch 'master' of https:#github.com/mahdiz/MpcLib.git"
git push     # Pushes the merged changes to remote
```

#### Working with remote
```bash
git remote -v
git remote rm <remote_name>                       # Remove a remote
git push -u <remote_name> <local_branch_name>     # Set the default remote
```

#### Show diff against the remote
```bash
git diff origin/master
git merge origin/master		# Accept remote changes
```

## Branches
Changes you make on a branch don't affect the master branch, so you're free to experiment and commit changes, safe in the knowledge that your branch won't be merged until it's ready to be reviewed by someone youâre collaborating with.

#### Existing branches
```bash
git branch				                        # See all local branches of this repo
git branch -a							        # See all branches (local and remote)
git checkout master                             # Switch branch: Sets the current branch to another branch
git checkout -b my_branch origin/my_branch		# Setup a local branch to track a remote branch origin/my_branch
```

#### Working with a new branch
```bash
git checkout -b [name_of_new_branch]        # Creates a new branch and sets the current branch to the new one
git push origin [name_of_new_branch]        # Push the branch to github to have a copy of the branch there
git push origin [name_of_new_branch]        # Push commits to the branch on github
git pull origin master			            # Update the current branch by merging the master branch changes
git push origin master			            # Push the 3d result to the master branch
git branch -d [name_of_branch]		        # Delete the branch from local filesystem
git push origin :[name_of_branch]	        # Delete the branch on github
```

#### Clone a specific branch
```bash
git clone -b mybranch --single-branch git://sub.domain.com/repo.git
```

## Stashing
When you don't want to do a commit of half-done work just so you can get back to a point later:
```bash
git stash          # To push a new stash onto your stack
git stash list     # To see which stashes youâve stored, you can use git stash list
```
The list will be shown as:
```bash
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log
```

```bash
git stash apply stash@{2}		# To reapply a stash
git stash drop					# To remove a stash
```

## Using hub
Hub provides extra features for github. To install hub on your machine:

1. Downaloding hub stand-alone executable from: https://github.com/github/hub/releases;
2. Run install.bat;
3. Relaunch the terminal.

To convert an issue to a pull request (attach commits to an existing issue and mark that issue as a Pull Request):
```bash
hub pull-request -i <ISSUE-NUMBER> -b <ORIGINAL_AUTHOR>:master -h <YOUR_USER_NAME>:my-changes
```
