# Squashing commits in a git feature branch into a single commit in master

So you finally finished the work on that new feature in your local branch and now, starring at the output of git log would like to commit something more intelligent than those 30 commits with message “typo correction” to appear in the master branch?  No problem with git.

What we are going to do will destroy commit history and will result in loss of your work if you screw up, so to be safe do the squashing in a separate branch

`git checkout -b squashing_branch`

To squash all commits in the feature branch into one, do

`git rebase -i master`

Your text editor will open with a file like this:
```
pick 3394ba1 commit msg 1
pick a4e4de9 commit msg 2
pick 2c57042 commit msg 3
pick 24386f9 commit msg 4
pick b86e92c commit msg 5
pick 08728d8 commit msg 6
```

Change it to look like
```
pick 3394ba1 commit msg 1
squash a4e4de9 commit msg 2
squash 2c57042 commit msg 3
squash 24386f9 commit msg 4
squash b86e92c commit msg 5
squash 08728d8 commit msg 6
```