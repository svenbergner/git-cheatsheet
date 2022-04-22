# git-cheatsheet
Collection of tips and tricks for using git

## Resetting files 
Reset all files to the HEAD of the branch

    git reset --hard HEAD

Reset a single file:

    git checkout HEAD -- path/to/file

Reset already committed changes:

    git reset --soft HEAD~1
    
## Delete local branches that have been removed from remote repo on fetch/pull
Delete stale branches in the local repo that no longer exist in the remote repo in each fetch/pull:

    git config --global fetch.prune true

Or manually add the following to your ~/.gitconfig:

    [fetch]
      prune = true
