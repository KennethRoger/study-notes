# GIT

Central VCS(Central version control system - SVN)
Distributed VCS(Distributed version control system - GIT)

- repository - A centralzied place to store and manage data, such as documents, software, or code.

## NEED HELP?

git help <verb>
git <verb> --help

# Three states with GIT

1. Working direcotry - Untracked and working file are found here
2. Staging Area - Organize what we want to be committed to our repository. Use git add to add files to staging area. You can use 'git reset <file/folder>' to remove files from the staging area
3. Committed files(.git directory) - Commit files to .git

# Git commands

# Common commands

git init
git status = To see file status
.gitignore
git log = To see the commits
git add
git reset = remove files from the staging area
git clone <url> <where to clone> = To clone a repository and use
git remote -v = Shows information to the repository
git branch -a = List all of the branches(In locally and remotely) in our repository
git diff = Shows the changes made
git pull origin master = Pull any changes made since the last time we pulled from that directory
git push origin master(origin is the name of our remote repository, master is branch we want to push to) = Push local refs to remote refs

## Commands for workflow for Git

git branch = List branch
git branch -a = List all of the branches(local and remote)
git branch <branchName> = Create a branch with the branchName
git checkout <branchName> = To start working on the branch branchName
git push -u origin <branchName> = (-u option tells git that we want to associate our local branch with the remote branch so in future you can specify git push or git pull without any branch details to specify)

### How to Merge a Branch

git checkout master = checkout to master branch
git pull origin master
git branch --merged = To see the branches that merged
git merge <branchName> = Merge a branch to the checkout branch (here master)
git push origin master

### Delete a branch

git branch -d <branchName> = To delete branch locally
git branch -a = List all branches remote and local
git push origin --delete <branchName> = To delete branch in remote repo

### Undo things

git checkout <branchname> = To checkout to branch where no changes are applied or if comitted you can do git checkout with the comitted id

git commit --amend -m "<message>" = Change the message in the last commit(The hash also changes)

git revert <hashOfCommit> - To revert to that hashes without changing the original commits

### To Make a commit happened in other branch(maybe you accidentely commited everything in master) to another branch

git log
git checkout <branchName>
git cherry-pick <hashOfTheLastCommit> = Adds the commit cherry-picked to the current working branch

### To reset the commits

**Three methods**
1. git reset --soft <hashOfCommit> = Soft will reset back to the specified commit but the changes are still made and seen in the staging area
2. git reset <hashOfCommit> (mixed reset) = mixed reset also resets the commit and the chages are also there but not in staging area. Found in the working directory
3. git reset --hard <hashOfCommit> = Reverts all of the tracked files to the original state but leaves any untracked files alone

git clean -df = to get rid of any untracked directory(with -d) and to get rid of any untracked files(-f)

### To try and retrieve the reset commit

git reflog = Shows most of the action you did

Go to the commit you accidently deleted with git checkout. You will then be in a detached head state. So create a new branch where the lost changes will be saved. 

### Stash changes instead of committing

git stash save "message like commit" =  Doesn't commit but saves the changes in a stash
git stash list = List the stashes
git stash apply <stashID> = Apply the stash changes
git stash pop = Removes the last stash and gives the changes back
git stash drop <stashID> = Removes the provided stash completely
git stash clear = Removes every stash
