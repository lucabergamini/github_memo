# github_memo
Git commands memo

## SETUP
``git config user.name <name>`` set the username for this repo

``git config alias.<al> "<cmd>"`` when calling ``git <al>`` the latter is replaced with ``<cmd>``

## CREATE AND SYNC
``git init .``  start a local repository in the current folder

``git init --bare repo.git``  start a local repositoryin the ``repo.git`` folder where only the ``.git`` is kept (no project file). Useful for starting a local remote

``git remote add <remote-name> <link>``  add a remote to the current local repo

``git remote set-url <remote-name> <link>`` reset the remote to another location (useful for changing origin from https to ssh)
## SANITY CHECKS
``git status``  print the status of the local repo (working and staged trees are shown)

``git diff <file>``  print the diff between the commited and working tree for the file (``--staged`` for seeing difference with staged tree)

``git log`` show latest commits

``git log --graph --all --oneline`` show a graph for all branches (both local and remote) with compressed commits

``git reflog`` show latest chamges in repo (checkouts, rebases, merges). Can be used to revert back to a commit when it's not pointed anymore

``git log -p <file>`` show commit where <file> was touched and which edits have been introduced
  
``git log -L <l-start>,<l-end>:<file>`` perform the check only in the lines range

``git blame -L <l-start>,<l-end> <file>`` shows who edited the lines range in <file> the LAST TIME

## UPDATE
``git fetch origin`` update local with new remote branches headers

``git pull origin <branch>``  this is actually equal to calling ``fetch`` followed by ``merge`` from local to remote branch 

``git add <file>``  add file to the staged aread for the current commit

``git commit -m ‘<message>’`` start a commit with the staged files

``git commit -am ‘<message>’`` add all versioned files and commit

``git push origin <branch>``  push data to the remote ``origin`` in the ``branch``

``git commit --amend`` add the new ``git add`` to previous commit
## BRANCH
``git branch``  show local branches (green for the one where head points to)

``git checkout <branch>``  switch to the selected local or remote branch

``git checkout -b <name>`` create a new local branch and checkout on it

``git checkout -b <name> <branch>`` create a new branch pointing to a remote existing one

``git branch -m <name>`` rename branch to ``name``

``git branch -d <name>`` remove the selected local branch

``git branch -D <name>``  remove the branch even if has unmerged commits with master

``git push -u origin <local branch>``  create a remote branch (with the same name) from ``local brach`` and push to it

``git rebase --onto master <b1> <b2>`` if ``b2`` was stemming from ``b1`` now it stems from master (and so it doesn't have all ``b1`` history)
## MERGE
``git merge <branch>``  merge ``branch`` to the one pointed by HEAD (run from local branch pointing to origin/master to get latest master edits)

``git checkout <branch> file1 file2 ...`` merge only selected file from local committed branch
## STASH
``git stash`` stash the current working tree in a FIFO stack and checkout the current branch (stash acts like a tag)

``git stash list`` show the list of stashed 

``git stash pop`` restore the top edit from the stash

## UNDO (without file)
``git reset --soft SHA`` **move branch** pointed from HEAD to SHA. Staged area is beyond commit, so we can `commit` again.

``git reset --mixed SHA`` **move branch** pointed from HEAD, update staging tree (index) with commit content (default of `reset`). Working area is beyond index and comit, so we can `add` again.

``git reset --hard SHA`` **move branch** pointed from HEAD, update staging tree (index) and working dir with commit content (**this can not be reversed for uncommited files!**). In practice, the repo looks like SHA now.

``git checkout SHA`` **move HEAD** to SHA, update index and working dirs **if it can be done cleanly**, otherwise stop (safe)

## UNDO (with file)
``git checkout file1 file2 ...`` **don't move HEAD**, update index and working tree version of the files (**this can not be reversed for uncommited files!**)

``git reset HEAD file1 file2 ...`` **don't move HEAD** update index tree version of the files

## TAGS
``git tag -a <tag_name>`` create an annotated tag named <tag_name> and fire up the editor to add a message (or `-m` and the message to do it inline)

``git push origin --tags`` push the tags to the remote named origin (this creates a distribution entry in GitHub)

## SUBMODULE
``git submodule add url_to/awesome_submodule.git path_to_awesome_submodule`` add the submodule to the current repo (note that this is a link to a commit, and it's NOT kept updated)

