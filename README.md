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

