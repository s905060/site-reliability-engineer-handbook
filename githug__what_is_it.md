# Githug – What is it?

Githug is an awesome program to help you improve your git skills. It contains several levels, that start off with some easy tasks and will go deeper with every level. At first you start with initializing a bare git repo, adding and committing files and configuring the .gitignore file. But then you get to remote repositories, stashing, rebasing, merging and reflogs. I found it very useful, to improve my knowledge about git and found myself reading the man files more often, getting to know some awesome details about git.

Githug is written in Ruby and maintained on Github. There are more levels to come, so my tutorial might be outdated by the time you read it. If you find any errors or missing levels, feel free to contact me.

###Installation

It is really easy to setup Githug. You can simply install it with the following command:

`gem install githug`

This should work on MacOS X and the common Linux distros. On Linux, you can simply use yum, apt-get or the packet-installation software, that your distro uses, to install gem.

###Solutions

As I had some trouble myself with some of those levels, I thought, that it might be helpful for other people to find a solution to the levels. I decided to make my solutions available, hoping they might help you. My approach of solving the levels might differ from your approach, but that’s the beauty of such problems. There often isn’t this one way to get things done. But without any further ado, here’s my solution.

###Level 1: init

A new directory, git_hug, has been created; initialize an empty repository in it.
```
cd git_hug/
git init
```

###Level 2: config

Set up your git name and email, this is important so that your commits can be identified.
```
git config ––global user.name „Tobias Kerst“
git config ––global user.email moc.erocsbot@tsrek.saibot
```

###Level 3: add

here is a file in your folder called README, you should add it to your staging area
Note: You start each level with a new repo. Don’t look for files from the previous one.
```
git add README
```

###Level 4: commit

The README file has been added to your staging area, now commit it.
```
git commit -m ‚Initial commit‘
```

###Level 5: clone

Clone the repository at https://github.com/Gazler/cloneme.
```
git clone https://github.com/Gazler/cloneme
```

###Level 6: clone_to_folder

Clone the repository at https://github.com/Gazler/cloneme to my_cloned_repo.
```
git clone https://github.com/Gazler/cloneme my_cloned_repo
```

###Level 7: ignore

The text editor ‘vim’ creates files ending in .swp (swap files) for all files that are currently open.We don’t want them creeping into the repository.Make this repository ignore .swp files.
```
vim .gitignore
Insert: *.swp
```

###LeveL 8: include

Notice a few files with the ‘.a’ extension.We want git to ignore all but the ‘lib.a’ file.
```
vim .gitignore
*.a
!lib.a
```

###Level 9: status

There are some files in this repository, one of the files is untracked, which file is it?
```
git status
```

You now have to simply insert the name of the untracked file: database.yml

###Level 10: number_of_files_commited

There are some files in this repository, how many of the files will be committed?
```
git status
```

Look, at those files, that are ready to be committed. Those are rubyfile1.rb and rubyfile4.rb

###Level 11: rm

A file has been removed from the working tree, however the file was not removed from the repository.Find out what this file was and remove it.
```
git status
git rm deleteme.rb
```

###Level 12: rm_cached

A file has accidentally been added to your staging area, find out which file and remove it from the staging area.NOTE Do not remove the file from the file system, only from git.
```
git status
git rm ––cached deleteme.rb
```

###Level 13: stash

You’ve made some changes and want to work on them later. You should save them, but don’t commit them.
```
git stash
```

###Level 14: rename

We have a file called oldfile.txt. We want to rename it to newfile.txt and stage this change.
```
git mv oldfile.txt newfile.txt
```

###Level 15: restructure

You added some files to your repository, but now realize that your project needs to be restructured.Make a new folder named src, and move all of the .html files into this folder.
```
mkdir src
git mv *.html src/
```

###Level 16: log

You will be asked for the hash of most recent commit.You will need to investigate the logs of the repository for this.
```
git log ––oneline
```

Enter the hash of the last commit that will be printed. For a nicer output, I chose the option ––oneline.

###Level 17: tag

We have a git repo and we want to tag the current commit with new_tag.
```
git tag new_tag
```

###Level 18: push_tags

There are tags in the repository that aren’t pushed into remote repository. Push them now.
```
git push ––tags
```

###Level 19: commit_amend

The README file has been committed, but it looks like the file forgotten_file.rb was missing from the commit.Add the file and amend your previous commit to include it.

git add forgotten_file.rb
git commit ––amend
Level 20: commit_in_future

Commit your changes with the future date (e.g. tomorrow).

git commit ––date=25.10.2015T22:23:23
In addition, the date part is accepted in the following formats: YYYY.MM.DD, MM/DD/YYYY and DD.MM.YYYY.

Level 21: reset

There are two files to be committed.The goal was to add each file as a separate commit, however both were added by accident.Unstage the file to_commit_second.rb using the reset command (don’t commit anything).

git reset HEAD to_commit_second.rb
Level 22: reset_soft

You committed too soon. Now you want to undo the last commit, while keeping the index.

git reset ––soft HEAD^
Level 23: checkout_file

A file has been modified, but you don’t want to keep the modification.Checkout the config.rb file from the last commit.

git checkout –– config.rb
Level 24: remote

This project has a remote repository.Identify it.

git remote
Level 25: remote_url

The remote repositories have a url associated to them.Please enter the url of remote_location.

git remote -v
Copy the url of the remote repo

Level 26: pull

You need to pull changes from your origin repository.

git pull origin master
Level 27: remote_add

Add a remote repository called origin with the url https://github.com/githug/githug

git remote add origin https://github.com/githug/githug
Level 28: push

Your local master branch has diverged from the remote origin/master branch. Rebase your commit onto origin/master and push it to remote.

git rebase origin/master master
git push origin master
Level 29: diff

There have been modifications to the app.rb file since your last commit.Find out which line has changed.

git diff
Explanation: You get the output, starting at line 23, which displays “erb :success”. The changes three lines below, so the changes are in line 26.

Level 30: blame

Someone has put a password inside the file config.rb find out who it was.

git blame `config.rb`
Level 31: branch

You want to work on a piece of code that has the potential to break things, create the branch test_code.

git branch test_code
Level 32: checkout

Create and switch to a new branch called my_branch.You will need to create a branch like you did in the previous level.

git checkout -b my_branch
Level 33: checkout_tag

You need to fix a bug in the version 1.2 of your app. Checkout the tag v1.2.

git checkout v1.2
Level 34: checkout_tag_over_branch

You need to fix a bug in the version 1.2 of your app. Checkout the tag v1.2 (Note: There is also a branch named v1.2).

git checkout tags/v1.2
Level 35: branch_at

You forgot to branch at the previous commit and made a commit on top of it. Create branch test_branch at the commit before the last.

git branch test_branch HEAD^
Level 36: delete_branch

You have created too many branches for your project. There is an old branch in your repo called ‘delete_me’, you should delete it.

git branch -d delete_me
Level 37: push_branch

You’ve made some changes to a local branch and want to share it, but aren’t yet ready to merge it with the ‘master’ branch.Push only ‘test_branch’ to the remote repository

git push origin test_branch
Level 38: merge

We have a file in the branch ‘feature’; Let’s merge it to the master branch.

git branch
git merge feature
I like check which branch I’m on by using git branch before I merge changes is. This helped me a lot in the past and I recommend you to do the same, in case you merge into the wrong branch.

Level 39: fetch

Looks like a new branch was pushed into our remote repository. Get the changes without merging them with the local repository.

git fetch origin
Level 40: repack

Optimise how your repository is packaged ensuring that redundant packs are removed.

git repack -d
Level 41: cherry-pick

Your new feature isn’t worth the time and you’re going to delete it. But it has one commit that fills in README file, and you want this commit to be on the master as well.

git branch
git checkout new-feature
git log ––oneline
git checkout master
git cherry-picker ca32a6d
I usually like to use ––onelinewhen I display the log in my Terminal. And as you might guess, the Hash I cherry-pick, belongs to the commit, that included the READMEfile. You can cherry-pick more than one file at a time, for further information you might want to read either this simple introduction or the documentation.

Level 42: grep

Your project’s deadline approaches, you should evaluate how many TODOs are left in your code

git grep TODO
After this, you just have to count the lines.

Level 43: rename_commit

Correct the typo in the message of your first (non-root) commit.

git log
git rebase -i ffc13cc
You need to look for the commit you want to rename, then replace the command pick with the word reword and then save the file. Another file opens, where you can rename your commit message.

Level 44: squash

You have committed several times but would like all those changes to be one commit.

git log
git rebase -i
Squash the first two commits into the newest one, which you want to pick.

Level 45: merge_squash

Merge all commits from the long-feature-branch as a single commit.

git merge ––squash long-feature-branch
git commit
Level 46: reorder

You have committed several times but in the wrong order. Please reorder your commits.

git log
git rebase -i
Then just switch the commit order, by copy-pasting the third commit after the second commit.

Level 47: bisect

A bug was introduced somewhere along the way.You know that running ruby prog.rb 5 should output 15.You can also run make test.What are the first 7 chars of the hash of the commit that introduced the bug.

git bisect start
ruby prog.rb 5 // Expect output 15
git bisect bad
git log
git checkout f608824 // Checkout the first commit
git bisect good
ruby prog.rb 5 // Test the output and decide, whether it’s good or bad
git bisect bad
//eat, sleep, check, repeat
Level 48: stage_lines

You’ve made changes within a single file that belong to two different features, but neither of the changes are yet staged. Stage only the changes belonging to the first feature.

git add -p
And then stage the lines manually.

Level 49: find_old_branch

You have been working on a branch but got distracted by a major issue and forgot the name of it. Switch back to that branch.

git reflog
git checkout solve_world_hunger // The last branch I was on before the last commit
Level 50: revert

You have committed several times but want to undo the middle commit.
All commits have been pushed, so you can’t change existing history.

git revert HEAD^1
Level 51: restore

You decided to delete your latest commit by running git reset ––hard HEAD^.(Not a smart thing to do.)You then change your mind, and want that commit back.Restore the deleted commit.

git reflog
git checkout bdc3d2b
Level 52: conflict

You need to merge mybranch into the current branch (master). But there may be some incorrect changes in mybranch which may cause conflicts. Solve any merge-conflicts you come across and finish the merge.

git branch // Just check, which branch you are one
git merge mybranch
git status
vim poem.txt
git add poem.txt
git commit
Edit the file so all the included lines disappear and only the one line that rhymes stays. You might be interested in this introduction.
