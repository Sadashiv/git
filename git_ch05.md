Distributed Git - Distributed Workflows
=======================================

Centralized Workflow
--------------------
![Centralized Workflow](images/centralized_workflow.png)


Distributed Git - Contributing to a Project
--------------------------------------------


Git commit message-> 
Capitalized, short (50 chars or less) summary
Further line
 -
 -

SmallTeam Workflow
------------------
![Centralized Workflow](images/small-team-flow.png)

git merge --squash branch -> Making single commit hash


Distributed Git - Maintaining a Project
---------------------------------------
$ git apply /tmp/patch-ruby-client.patch

Applying a Patch with am<br/>
If the contributor is a Git user and was good enough to use the format-patch command to generate their patch,<br/>
then your job is easier because the patch contains author information and a commit message for you. If you can,<br/>
encourage your contributors to use format-patch instead of diff to generate patches for you. You should only<br/>
have to use git apply for legacy patches and things like that.<br/>

git am 0001-limit-log-function.patch<br/>

Checking Out Remote Branches
----------------------------
git checkout -b rubyclient jessica/ruby-client

Rebasing and Cherry-Picking Workflows
-------------------------------------
A cherry-pick in Git is like a rebase for a single commit. It takes the patch that was introduced in a commit<br/>
and tries to reapply it on the branch you’re currently on.<br/>
$ git cherry-pick e43a6<br/>


Rerere(“reuse recorded resolution”)
-----------------------------------
If you’re doing lots of merging and rebasing, or you’re maintaining a long-lived topic branch,<br/>
Git has a feature called “rerere” that can help.<br/>
Rerere stands for “reuse recorded resolution” — it’s a way of shortcutting manual conflict resolution.<br/>
When rerere is enabled, Git will keep a set of pre- and post-images from successful merges, and if it<br/>
notices that there’s a conflict that looks exactly like one you’ve already fixed, it’ll just use the fix<br/>
from last time, without bothering you with it.<br/>

$ git config --global rerere.enabled true<br/>
