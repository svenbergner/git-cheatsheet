# git-cheatsheet
Collection of tips and tricks for using git

## Contents
 - [Resetting files](#resetting-files)
 - [Reset a branch](#reset-a-branch)
 - [Delete local branches that have been removed from remote repo on fetch/pull](#delete-local-branches-that-have-been-removed-from-remote-repo-on-fetchpull)
 - [Empty folders](#empty-folders)
   - [Adding an empty folders to your repo](#adding-an-empty-folders-to-your-repo)
   - [Keep a folder empty](#keep-a-folder-empty)
 - [Comparing](#comparing)
   - [Comparing a file from two different branches](#comparing-a-file-from-two-different-branches)
 - [Settings](#settings)
   - [Global .gitconfig](#global-gitconfig)
   - [Using a global .gitignore file](#using-a-global-gitignore-file)

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

    git reset --hard origin/branch_to_overwrite
    
*Important*: All tracked local changes will be lost. All local commits that haven't been pushed will be lost. Untracked files will not be affected.

First, run a fetch to update all origin/\<branch\> refs to latest:

    git fetch --all
    
Backup your current branch:

    git branch backup-<branch_name>

Then reset to origin:

    git reset --hard origin/<branch_name>
    
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

    git stash
And then to reapply these uncommitted changes:

    git stash pop    

### Undo local changes
Works only on tracked files with uncommited changes.

To undo all local changes:

    git checkout -- .

To undo changes of a specific file:

    git checkout -- <file>

    
## Cleanup local repos

### Remove untracked files
Delete all untracked files and directories, including ignored ones:

    git clean -d -x -f

### Delete local branches
Delete a local branch that is completely merged to the server:

    git branch -d feature_branch_name

Delete a local branch that is not up to date:

    git branch -D feature_branch_name

### Manually Delete local branches that have been removed from remote repo on fetch/pull

    git fetch --prune origin

### Always delete local branches that have been removed from remote repo on fetch/pull
Delete stale branches in the local repo that no longer exist in the remote repo in each fetch/pull:

    git config --global fetch.prune true

Or manually add the following to your ~/.gitconfig:

    [fetch]
      prune = true

## Comparing
What do you need to know about comparing things in Git?

### Comparing a file from two different branches
If you want to know the difference of the content of a file between two branches:

    git diff feature_branch main -- file.name

## Empty folders
### Adding an empty folders to your repo
Git only cares about files. Therefore you can't add an empty folder to your repo. As a convention developers started to add a *.gitkeep* file inside the folder.
    
    touch .gitkeep
    
Otherwise add a Readme.md file to the folder with information about it.

### Keep a folder empty
If you want an empty folder and make sure it stays 'clean' for Git, create a .gitignore containing the following lines within:

    # .gitignore sample
    # Ignore all files in this dir...
    *

    # ... except for this one.
    !.gitignore

If you desire to have only one type of files being visible to Git, here is an example how to filter everything out, except .gitignore and all .txt files:

    # .gitignore to keep just .txt files
    # Filter everything...
    *

    # ... except the .gitignore...
    !.gitignore

    # ... and all text files.
    !*.txt

## Settings
Useful entries for your .gitconfig file

### Using Windows Git Credential Manager on WSL

    git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"

### Automatically setup remote branch on pushing
    git config --global --add --bool push.autoSetupRemote true
    
### Global .gitconfig
An example .gitconfig file which can be used for all git repos on the system if it is placed in your home directory.

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

### Using a global .gitignore file

    [core]
        excludesfile = ~/.gitignore
