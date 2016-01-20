# Squashing commits in a git feature branch into a single commit in master

So you finally finished the work on that new feature in your local branch and now, starring at the output of git log would like to commit something more intelligent than those 30 commits with message “typo correction” to appear in the master branch?  No problem with git.

What we are going to do will destroy commit history and will result in loss of your work if you screw up, so to be safe do the squashing in a separate branch

`git checkout -b squashing_branch`

To squash all commits in the feature branch into one, do

`git rebase -i master`

Your text editor will open with a file like this: