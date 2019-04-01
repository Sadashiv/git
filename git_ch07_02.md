Workflow
--------
![Git workflow](images/reset-workflow.png)
git reset HEAD
git reset --soft commitid
git reset --mixed commitid
git reset --hard commitid

So, assume we run git reset file.txt. This form (since you did not specify a commit SHA-1 or branch,
and you didn’t specify --soft or --hard) is shorthand for git reset --mixed HEAD file.txt, which will:

| Commit Level             | HEAD  | Index | Workdir  | WD Safe?|
|--------------------------|-------|-------|----------|---------|
| reset --soft [commit]    | REF   |  NO   |   NO     |  YES    |
| reset [commit]           | REF   |  YES  |   NO     |  YES    |
| reset --hard [commit]    | REF   |  YES  |   YES    |  NO     |
| checkout <commit>        | HEAD  |  YES  |   YES    |  YES    |
| File Level               |       |       |          |         |
| reset [commit] <paths>   | NO    |  YES  |   NO     |  YES    |
| checkout [commit] <paths>| NO    |  YES  |   YES    |  NO     |

Advanced merging
----------------
Xignore-all-space or -Xignore-space-change. The first option ignores whitespace completely when comparing lines,<br/>
the second treats sequences of one or more whitespace characters as equivalent.<br/>

$ git merge -Xignore-space-change whitespace<br/>
$ git diff --ours<br/>
$ git diff --theirs<br/>

Reverse the commit
------------------
If moving the branch pointers around isn’t going to work for you, Git gives you the<br/>
option of makinga new commit which undoes all the changes from an existing one. Git<br/>
calls this operation a “revert”, and in this particular scenario, you’d invoke it like this:<br/>

$ git revert commitid (Commit changes will be available and going to revert changes)<br/>
  Later same commitid can used to do cherry-pick or get changes<br/>

$ git ls-files -u --> to see the conflicted files and the before, left and right versions:<br/>

Git Tools - Debugging with Git
------------------------------
$ git blame -L 69,75 git_ch07.md
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 69) Creating a Branch from a Stash<br/>
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 70) ------------------------------<br/>
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 71) $ git stash branch testchanges<br/><br/>
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 72) 
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 73) Cleaning your Working Directory<br/>
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 74) -------------------------------<br/>
  de62db0c (Sadashiv H 2019-03-29 21:40:22 +0530 75) git clean -fd<br/> -f => force -d => directory -n => dry run<br/>


$ git blame -C -L 69,75 git_ch07.md -> -C to initial commit code

Binary Search
-------------
$ git bisect start<br/>
$ git bisect good<br/>
$ git bisect bad v1.1<br/>

Git Tools - Submodules
----------------------
It often happens that while working on one project, you need to use another<br/>
project from within it.Perhaps it’s a library that a third party developed or<br/>
that you’re developing separately and using in multiple parent projects.<br/>

$ git submodule add https://github.com/Sadashiv/rpm
$ git status
$ cat .gitmodules 
  [submodule "jenkins"]
     path = jenkins
     url = https://github.com/Sadashiv/jenkins

$ git diff --cached --submodule

Cloning a Project with Submodules
---------------------------------

$ git clone https://github.com/Sadashiv/git
$ git submodule init

$ git clone --recurse-submodules https://github.com/chaconinc/MainProject
#clone with recurse modules
$ git submodule update --remote jenkins
#Git will by default try to update all of your submodules when you run git submodule update --remote

$ git submodule update --remote --merge
$ git submodule update --remote --rebase
$ git push --recurse-submodules=check
$ git submodule foreach 'git stash'

$ git submodule foreach 'git checkout -b featureA'

Issues with Submodules
----------------------
Using submodules isn’t without hiccups, however.<br/>

For instance switching branches with submodules in them can also be tricky.<br/>
If you create a new branch, add a submodule there, and then switch back to<br/>
a branch without that submodule, you still have the submodule directory as an untracked directory:<br/>

Git Tools - Bundling
--------------------
Though we’ve covered the common ways to transfer Git data over a network<br/>
(HTTP, SSH, etc), there is actually one more way to do so that is not<br/>
commonly used but can actually be quite useful.<br/>

$ git bundle create gitrepo.bundle HEAD git
  Counting objects: 230, done.
  Delta compression using up to 8 threads.
  Compressing objects: 100% (134/134), done.
  Writing objects: 100% (230/230), 243.54 KiB | 20.29 MiB/s, done.
  Total 230 (delta 119), reused 154 (delta 95)

$ git clone gitrepo.bundle repo
  cd repo
  git log

$ git bundle verify ./gitrepo.bundle
  The bundle contains these 2 refs:
  c38911da4f5316267633bc48124642e0c6456e49 HEAD
  c38911da4f5316267633bc48124642e0c6456e49 refs/heads/git
  The bundle records a complete history.
  ./gitrepo.bundle is okay

Note: git bundle can be really useful for sharing or doing network-type<br/>
operations when you don’t have the proper network or shared repository to do so.<br/>

Replace
-------
The commit-tree command is one of a set of commands that are commonly<br/>
referred to as plumbing commands. These are commands that are not gen<br/>
erally meant to be used directly, but instead are used by other Git c<br/>
ommands to do smaller jobs. On occasions when we’re doing weirder thi<br/>
ngs like this, they allow us to do really low-level things but are not<br/>
meant for daily use. You can read more about plumbing commands in Plu</br>
mbing and Porcelain<br/>

Git Tools - Credential Storage
------------------------------

ssh remote can be used with or without passphrase<br/>
$ git config credential.helper store<br/>
$ vim ~/.git-credentials<br/>
$ git push git git<br/>
$ cat ~/.git-credentials<br/>


