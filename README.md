# github_memo
Github commands memo

## CREATE AND SYNCH
``git init .``  start a local repository in the current folder

``git remote add origin <link>``  set the new origin for the local repo to point to

``git remote set-url origin <link>`` reset the origin (useful for setting origin to ssh)
## SANITY CHECKS
``git status``  print the status of the local repo

``git remote update``  update the info about the pointed remote branch

``git diff <file>``  print the diff between the versions of the file

## UPDATE
``git pull origin <branch>``  pull data from the specified branch (e.g. Master)

``git add <file>``  add file to the current commit

``git commit -m ‘<message>’`` start a commit with the added files

``git push origin <branch>``  push data to the remote branch

``git commit --amend`` add the new ``git add`` to previous commit
## BRANCH
``git branch``  show working branch

``git branch <name>``  create a new local branch

``git checkout -b <name>`` create a new local branch and checkout on it

``git branch <name> <branch>``  create a new branch pointing to a remote one

``git branch -d <name>`` remove the selected local branch

``git branch -D <name>``  remove the branch even if forwards master

``git checkout <branch>``  switch to the selected local or remote branch

``git checkout <status hash>``  detach head to the selected hash

``git push -u origin <branch>``  create a remote branch from the local one
## MERGE
``git merge <branch>``  merge the selected branch to the current (run from local branch pointing to origin/master to get latest master edits)

``git checkout <branch> file1 file2 ...`` merge only selected file from local committed branch
## STASH
``git stash`` stash the current edit in a FIFO stack and checkout the current branch

``git stash list`` show the list of stashed 

``git stash pop`` restore the top edit from the stash
## UNDO
``git reset HEAD~`` destroy latest unpushed commit

``git checkout file1 file2 ...`` revert to latest commit status
## SUBMODULE
``git submodule add url_to/awesome_submodule.git path_to_awesome_submodule`` add the submodule to the current repo (note that this is a link to a commit, and it's NOT kept updated)

