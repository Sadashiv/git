Git Tips and tricks
===================

Get a latest commit from branch
---------------------------------
cat .git/refs/heads/git
be98beb57eed7e842dbbcc19afc1a340dc7a2f61
or
git rev-parse gitolite (gitolite is branch)
be98beb57eed7e842dbbcc19afc1a340dc7a2f61

git revert and git rebase 
-------------------------
git revert commitid -> Removes the data associated with commit and creates new commitid2
Again you want to bring changes back use commitid git cherry-pick commitid or git revert commitid2


git checkout master -- linux/ #checkout the sinle file of multiple file from master branch
git diff master test path_to_file_or_folder > diff.txt #Diff between two branches

The lesson here is that if you’re going to push changes, make a bare repository clone with git clone --bare.

Git add content selectively
--------------------------
git add -p

git stash drop [<stash-name>] --> delete the stash
---------------------------------------------------

Delete all the local tag:<br/>
$ git tag -l | xargs git tag -d<br/>
Delete all the Remote tag:<br/>
git tag -l | xargs -n 1 git push --delete origin<br/>

$ git worktree add /home/sadashiv/worktree<br/>
Preparing /home/sadashiv/worktree (identifier worktree)<br/>
HEAD is now at 62d8d44 Inlcude the cleanup old objects<br/>
$ git worktree add /home/sadashiv/worktreem master<br/>
Preparing /home/sadashiv/worktreem (identifier worktreem)<br/>
HEAD is now at 95e0644 Added README<br/>
$ git worktree list<br/>
/home/sadashiv/gittraining  62d8d44 [heads/git]<br/>
/home/sadashiv/worktree     62d8d44 [worktree]<br/>
/home/sadashiv/worktreem    95e0644 [master]<br/>

$ ls .git/worktrees/<br/>
  gitworktree  issufixworktreem

$ cat .git/worktrees/gitworktree/gitdir<br/>
/home/sadashiv/worktree/.git<br/>

$ ls .git/worktrees/worktree/<br/>
commondir  gitdir  HEAD  index	logs  ORIG_HEAD<br/>

$ git worktree remove ../worktree<br/>
<b>Git worktree can't be used with submodule</b><br/>

git pull and rebase
fast forword

Three way merge
refrance/objects data

"""
git shortlog

Sadashiv Hanamant (1):
      Added the basic folder structure

Sadshiv (4):
      Removed the pdf's
      Added the buildout for windows
      Moved the file
      Delete file
"""
git log --author=Sadashiv

git log --stat # to see the number of files added or deleted, Number of line added or deleted
git gc --prune --agressive

