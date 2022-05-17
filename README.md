# git-cheatsheet
Collection of tips and tricks for using git

## Resetting files 
Reset all files to the HEAD of the branch

    git reset --hard HEAD

Reset a single file:

    git checkout HEAD -- path/to/file

Reset already committed changes:

    git reset --soft HEAD~1
    
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

    git checkout master
    git branch new-branch-to-save-current-commits
    git fetch --all
    git reset --hard origin/master
    
After this, all of the old commits will be kept in new-branch-to-save-current-commits.

Uncommitted changes will be lost. Make sure to stash and commit anything you need.

    git stash
And then to reapply these uncommitted changes:

    git stash pop    
    
    

## Delete local branches that have been removed from remote repo on fetch/pull
Delete stale branches in the local repo that no longer exist in the remote repo in each fetch/pull:

    git config --global fetch.prune true

Or manually add the following to your ~/.gitconfig:

    [fetch]
      prune = true

## Useful entries for your .gitconfig file

### Using a global .gitignore file

    [core]
        excludesfile = ~/.gitignore

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
