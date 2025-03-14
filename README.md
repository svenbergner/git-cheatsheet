# Git CheatSheet
Collection of Tips and Tricks for using git

## Contents
 - [Preview changes before pulling](#preview-changes-before-pulling)
 - [Diff a file between two branches](#diff-a-file-between-two-branches)
 - [Undo last commit](#undo-last-commit)
 - [Resetting files](#resetting-files)
 - [Reset a branch](#reset-a-branch)
   - [Undo local changes](#undo-local-changes)
   - [Restore the Staged Area](#restore-the-staged-area)
 - [Stash](#stash)
   - [Create Stash with a Single File/Path](#create-stash-with-a-single-filepath)
   - [Create Stash including untracked files](#create-stash-including-untracked-files)
 - [Cleanup local repos](#cleanup-local-repos)
   - [Clean All (tracked and untracked files)](#clean-all-tracked-and-untracked-files)
   - [Remove untracked files](#remove-untracked-files)
   - [Delete local branches](#delete-local-branches)
   - [Manually Delete local branches that have been removed from remote repo on fetch or pull](#manually-delete-local-branches-that-have-been-removed-from-remote-repo-on-fetch-or-pull)
   - [Always delete local branches that have been removed from remote repo on fetch or pull](#always-delete-local-branches-that-have-been-removed-from-remote-repo-on-fetch-or-pull)
   - [Optimize git repository performance](#optimize-git-repository-performance)
- [Comparing](#comparing)
     - [Comparing a file from two different branches](#comparing-a-file-from-two-different-branches)
 - [Empty folders](#empty-folders)
   - [Adding an empty folders to your repo](#adding-an-empty-folders-to-your-repo)
   - [Keep a folder empty](#keep-a-folder-empty)
 - [Push Force safely](#push-force-safely)
 - [Getting Repo Information](#getting-repo-information)
   - [Number of commits in the whole repo](#number-of-commits-in-the-whole-repo)
   - [Number of commits in a specific revision](#number-of-commits-in-a-specific-revision)
 - [Settings](#settings)
   - [Reuse Recorded Resolution](#reuse-recorded-resolution)
   - [Using Windows Git Credential Manager on WSL](#using-windows-git-credential-manager-on-wsl)
   - [Automatically setup remote branch on pushing](#automatically-setup-remote-branch-on-pushing)
   - [Global .gitconfig](#global-gitconfig)
   - [Using a global .gitignore file](#using-a-global-gitignore-file)
   - [Backup repository on a zip file](#backup-repository-on-a-zip-file)
   - [Show top contributors](#show-top-contributors)
   - [Move back to the previous branch](#move-back-to-the-previous-branch)

## Preview changes before pulling

Start with a git fetch to update the state of the remote branches on your local repo.

```shell
    git fetch
```

Show the log entries between your last common commit and the origin's master branch
```shell
    git log HEAD..origin/master
```

Show the diffs for each patch
```shell
    git log -p HEAD..origin/master
```

Show one liner log
If you just run git log, you will notice that the output is quite verbose. If you only want to see the hashes
```shell
    git log --oneline
```

Show history of a specific file
To check all the changes made on a specific file over time, run
```shell
    git log --follow file.txt
```

Show the diffs for a single diff
```shell
    git diff HEAD...origin/master (n.b. three dots not two) 
```

If you're not prepared to do a pull and merge in all the remote commits, you can cherry pick the specific remote commits you want.

```shell
    git cherry-pic <commit-ish>
```

A git pull will merge in the rest of the commits.
```shell
    git pull
```

## Diff a file between two branches
Show all differences of a single file between two branches.

```shell
    git diff branch1..branch2 -- filename.ext
```

## Show all branches that are completely merged
Gives you a list of all branches which are completely merged to the current branch. Though they can savely removed.

```shell
    git branch --merged
```

## Undo last commit
Undos the last local commit but preserves the changes
```shell
    git reset --soft HEAD~
```

## Resetting files 
Reset all files to the HEAD of the branch
```shell
    git reset --hard HEAD
```
Reset a single file:
```shell
    git checkout HEAD -- path/to/file
```

Reset already committed changes:
```shell
    git reset --soft HEAD~1
```

## Reset a branch
Reset your local branch to a remote branch
```shell
    git reset --hard origin/branch_to_overwrite
```

*Important*: All tracked local changes will be lost. All local commits that haven't been pushed will be lost. Untracked files will not be affected.

First, run a fetch to update all origin/\<branch\> refs to latest:
```shell
    git fetch --all
```

Backup your current branch:
```shell
    git branch backup-<branch_name>
```

Then reset to origin:
```shell
    git reset --hard origin/<branch_name>
```

Maintain current local commits
It's worth noting that it is possible to maintain current local commits by creating a branch from master before resetting:
```shell
    git checkout master
    git branch new-branch-to-save-current-commits
    git fetch --all
    git reset --hard origin/master
```
    
After this, all of the old commits will be kept in new-branch-to-save-current-commits.

Uncommitted changes will be lost. Make sure to stash and commit anything you need.
```shell
    git stash
```

And then to reapply these uncommitted changes:
```shell
    git stash pop    
```

### Undo local changes
Works only on tracked files with uncommited changes.

To undo all local changes:
```shell
    git checkout -- .
```

To undo changes of a specific file:
```shell
    git checkout -- <file>
```

### Restore the Staged Area
A.K.A. unstage files
```shell
    git restore --staged .
```

## Stash
If you need to cleanup your repo temporarily you have some possibilities to stash your work.

### Create Stash with a Single File/Path
Since git 2.13 you can save a specific file or path to the stash
```shell
    git stash push -u file.txt
```

### Create Stash including untracked files
When running git stash, git will include only the tracked files. If you would like to include untracked files you need to add a flag
```shell
    git stash --include-untracked
```

## Cleanup local repos

### Clean All (tracked and untracked files)
```shell
    git clean -df
```

### Remove untracked files
Delete all untracked files and directories, including ignored ones:
```shell
    git clean -d -x -f
```

### Delete local branches
Delete a local branch that is completely merged to the server:

```shell
    git branch -d feature_branch_name
```

Delete a local branch that is not up to date:

```shell
    git branch -D feature_branch_name
```

### Manually Delete local branches that have been removed from remote repo on fetch or pull

```shell
    git fetch --prune origin
```

### Always delete local branches that have been removed from remote repo on fetch or pull
Delete stale branches in the local repo that no longer exist in the remote repo in each fetch/pull:

```shell
    git config --global fetch.prune true
```

Or manually add the following to your ~/.gitconfig:
```
    [fetch]
      prune = true
```

### Optimize git repository performance
If you repository slows down and performance is an issue, you can just run the garbage collector
```shell
    git gc --prune=now
```

## Comparing
What do you need to know about comparing things in Git?

### Comparing a file from two different branches
If you want to know the difference of the content of a file between two branches:
```shell
    git diff feature_branch main -- file.name
```

## Empty folders

### Adding an empty folders to your repo
Git only cares about files. Therefore you can't add an empty folder to your repo. As a convention developers started to add a *.gitkeep* file inside the folder.
```shell
    touch .gitkeep
```

Otherwise add a Readme.md file to the folder with information about it.

### Keep a folder empty
If you want an empty folder and make sure it stays 'clean' for Git, create a .gitignore containing the following lines within:
```
    # .gitignore sample
    # Ignore all files in this dir...
    *

    # ... except for this one.
    !.gitignore
```

If you desire to have only one type of files being visible to Git, here is an example how to filter everything out, except .gitignore and all .txt files:
```
    # .gitignore to keep just .txt files
    # Filter everything...
    *

    # ... except the .gitignore...
    !.gitignore

    # ... and all text files.
    !*.txt
```

## Push Force safely
Forcing a push has several problems especially when you are working in a team. Here's the safe alternative:
```shell
    git push --force-with-lease
```

## Getting Repo Information

### Number of commits in the whole repo

```shell
    git rev-list --count --all
```

### Number of commits in a specific revision

```shell
    git rev-list --count <revesion>
```

## Settings
Useful entries for your .gitconfig file

### Reuse Recorded Resolution
If you're rebasing or cherry-picking a lot and you've ever run into the same conflict more than once, 
you can turn on a feature where Git memorizes the conflict and the resolution to it. If it ever sees 
that same conflict again, it will automatically re-solve it for you.

You can turn it on with the config setting rerere.enabled and you can further ask it to automatically 
stage it for you with rerere.autoUpdate
```shell
    git config --global rerere.enabled true
    git config --global rerere.autoUpdate true
```

### Using Windows Git Credential Manager on WSL
```shell
    git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```

### Automatically setup remote branch on pushing
```shell
    git config --global --add --bool push.autoSetupRemote true
```
    
### Global .gitconfig
An example .gitconfig file which can be used for all git repos on the system if it is placed in your home directory.

```
    [user]
	    name = <UserName>
	    email = <Email-Address>
    [credential]
	    helper = manager-core
    [diff]
	    tool = bc4
    [difftool]
	    prompt = false
    [difftool "vsdiffmerge"]
        cmd = \"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\CommonExtensions\\Microsoft\\TeamFoundation\\Team Explorer\\vsdiffmerge.exe\" \"$LOCAL\" \"$REMOTE\" //t
        keepBackup = false

    [difftool "beyondcompare"]
        cmd = \"C:\\Program Files\\Beyond Compare 4\\BCompare.exe\" \"$LOCAL\" \"$REMOTE\"
        keepBackup = false

    [merge]
        tool = bc4
    [mergetool]
        prompt = false
    [mergetool "vsdiffmerge"]
        cmd = \"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\Common7\\IDE\\CommonExtensions\\Microsoft\\TeamFoundation\\Team Explorer\\vsdiffmerge.exe\" \"$REMOTE\" \"$LOCAL\" \"$BASE\" \"$MERGED\" //m
        keepBackup = false
        trustExitCode = true

    [mergetool "beyondcompare"]
        cmd = \"C:\\Program Files\\Beyond Compare 4\\BCompare.exe\" \"$REMOTE\" \"$LOCAL\" \"$BASE\" \"$MERGED\"
        keepBackup = false
        trustExitCode = true
    [core]
        excludesfile = ~/.gitignore
    [difftool "bc4"]
        path = C:\\Program Files\\Beyond Compare 4\\BCompare.exe
    [mergetool "bc4"]
        path = C:\\Program Files\\Beyond Compare 4\\BCompare.exe
    [credential "https://<credential server url>"]
    	provider = generic
```

### Using a global .gitignore file
```
    [core]
        excludesfile = ~/.gitignore
```

## Backup repository on a zip file
```shell
    git archive --format=zip --output=output.zip
```

## Show top contributors
```shell
    git shortlog -sn
```

## Move back to the previous branch
```shell
    git checkout -
```
