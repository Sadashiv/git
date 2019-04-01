Git Tools - Revision Selection
==============================

$ git log --abbrev-commit --pretty=oneline

Generally, eight to ten characters are more than enough to be unique within a project.<br/>
For example, as of February 2019, the Linux kernel (which is a fairly sizable project)<br/>
has over 875,000 commits and almost seven million objects in its object database, with<br/>
no two objects whose SHA-1s are identical in the first 12 characters.<br/>


RefLog Shortnames
-----------------

One of the things Git does in the background while you’re working away is keep a “reflog” —<br/>
a log of where your HEAD and branch references have been for the last few months.<br/>

You can see your reflog by using git reflog:<br/>

$ git reflog
c05b6db (HEAD -> git, git/git) HEAD@{0}: commit: Update branching and rebasing<br/>
d057607 HEAD@{1}: commit (amend): Include images in place of required<br/>
7758ab4 HEAD@{2}: commit (amend): Include images in place of required<br/>
7e1372f HEAD@{3}: commit: Include images in place of required<br/>
05be734 HEAD@{4}: commit: Include the image<br/>
Every time your branch tip is updated for any reason, Git stores that information for you in this<br/>
temporary history. You can use your reflog data to refer to older commits as well<br/>

$ git show HEAD@{5}<br/>
It’s important to note that reflog information is strictly local<br/>
 
Double dot
----------

git log master..git<br/>
Commits(git) which are not present in master or different one<br/>

Triple Dot
----------
The last major range-selection syntax is the triple-dot syntax, which specifies all the commits<br/>
that are reachable by either of two references but not by both of them.<br/>


Git Tools - Interactive Staging
-------------------------------
$ git add -i<br/>

Staging Patches
---------------
git add -p

Git Tools - Stashing and Cleaning
---------------------------------
Often, when you’ve been working on part of your project, things are in a messy state<br/>
and you want to switch branches for a bit to work on something else. The problem is,<br/>
you don’t want to do a commit of half-done work just so you can get back to this point<br/>
later. The answer to this issue is the git stash command.<br/>

As of late October 2017, there has been extensive discussion on the Git mailing list,<br/>
wherein the command git stash save is being deprecated in favour of the existing alternative git stash push.<br/>

$ git stash
stash@{0}: WIP on git: 5f2f854 Newline with br and p to check in browser<br/>
stash@{1}: WIP on git: e2df905 Include the git basics<br/>

$ git stash list<br/>
$ git stash apply<br/>

Creating a Branch from a Stash
------------------------------
$ git stash branch testchanges<br/>

Cleaning your Working Directory
-------------------------------
git clean -fd<br/> -f => force -d => directory -n => dry run

Git Tools - Signing Your Work
-----------------------------
$ git tag -s v1.5 -m 'my signed 1.5 tag'<br/>
$ git show v1.5<br/>

Git Tools - Searching
---------------------
$ git grep "print"<br/>

git grep --count git<br/>
README.md:8<br/>
git.txt:25<br/>
git_ch01.md:21<br/>
git_ch02.md:58<br/>
git_ch03.md:14<br/>
git_ch07.md:20<br/>

Git Log Searching
-----------------
git log -S update --oneline<br/>
be98beb Added project to repo<br/>

Line Log Search
---------------
git log -L :git:git.txt<br/>
be98beb 8 days ago <Sadashiv H>  Added project to repo (Thu Mar 21 15:52:52 2019 +0530)<br/>

diff --git a/git.txt b/git.txt<br/>
--- /dev/null<br/>
+++ b/git.txt<br/>
@@ -0,0 +1,1 @@<br/>
+git co master -- linux/* 
checkout the sinle file of multiple file from other branch<br/>


$ git commit --amend --no-edit<br/>

Changing Multiple Commit Messages
---------------------------------
git rebase -i HEAD~3 # Last three commits to be append<br/>
pick f7f3f6d changed my name a bit<br/>
squash 310154e updated README formatting and added blame<br/>
squash a5f4a0d added cat-file<br/>


Git Tools - Reset Demystified
------------------------------

Git as a system manages and manipulates three trees in its normal operation:<br/>

------------------------
Tree                Role
------------------------
HEAD                Last commit snapshot, next parent
Index               Proposed next commit snapshot
Working Directory   Sandbox


git cat-file -p HEAD
tree a8f5577ded01620c2b4864cde6dc6e52f5e5af53
parent d057607d649af93f90150082830fb1baed67bde5
author Sadashiv H <sadamsrit@gmail.com> 1553754799 +0530
committer Sadashiv H <sadamsrit@gmail.com> 1553754799 +0530

Update branching and rebasing

git ls-files -s
100644 a861890567b0212efd0cf4bfdca81a7441ed893d 0	README.md
100644 c4172086bf29e24255eef93c9db223b196f62534 0	git.txt
100644 32a6d4345b2737fa9512a309ecbe9a70ed16513e 0	git_ch01.md

