Git Branching
=============

Branch and snapshot
-------------------
![A branch and its commit history](images/branch-and-history.png)

Creating a new branch
---------------------
git branch new_branch_name<br/>
git chekcout -b new_branch_name<br/>

![A branch and its commit history](images/branch-and-history.png)


Git Branching - Basic Branching and Merging
-------------------------------------------
git chekcout -b issue1<br/>
git merge issue1<br/>
Merge conflict resolve<br/>
git pull origin master - Pull the change and merge better way to do it<br/>
git fetch origin - Bring changes from remote to local
git rebase origin/master - Merge changes to locally Don't do it for public repo<br/>

git fetch origin - Bring changes from remote to local<br/>
git merge origin/master - Merge changes to locally safe way<br/>

git branch -d testing - Delete branch<br/>
git branch -D testing - Delete branch<br/>
git branch --merged<br/>
git branch --no-merged<br/>

Git Branching - Branching Workflows
-----------------------------------
![Git Branching - Branching Workflows](images/topic-branches-1.png)


Git Branching - Remote Branches
-------------------------------
![Git Branching - Remote Branches](images/remote-branches-1.png)

Git Branching - Rebasing
------------------------
you can take the patch of the change that was introduced in C4 and reapply it on top of C3. In Git,<br/>
this is called rebasing. With the rebase command, you can take all the changes that were committed<br/>
on one branch and replay them on a different branch.<br/>

![Git Branching - Remote Branches](images/basic-rebase-3.png)

The Perils of Rebasing
----------------------
Ahh, but the bliss of rebasing isn’t without its drawbacks, which can be summed up in a single line:<br/>
Do not rebase commits that exist outside your repository and people may have based work on them.<br/>
If you follow that guideline, you’ll be fine. If you don’t, people will hate you, and you’ll be scorned by friends and family.

Rebase vs Merge
---------------
In general the way to get the best of both worlds is to rebase local changes you’ve made but haven’t shared<br/>
yet before you push them in order to clean up your story, but never rebase anything you’ve pushed somewhere.<br/>

 git rebase master
