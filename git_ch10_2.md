Git Internals - Git References
==============================

Git References
--------------
$ git update-ref refs/git 57cf1307c0eedb69bb5df7426bbe925957320d12<br/>

**
When you run commands like git branch <branch>, Git basically runs that update-ref command<br/>
to add the SHA-1 of the last commit of the branch you’re on into whatever new reference you want to create.<br/>

The HEAD
---------
The question now is, when you run git branch <branch>, how does Git know the SHA-1 of the last commit? The answer is the HEAD file.

$ cat .git/HEAD
ref: refs/heads/git
The HEAD file is a symbolic reference to the branch you’re currently on.<br/>

Update the symbolic-ref
$ git symbolic-ref HEAD
$ git symbolic-ref HEAD refs/heads/git

Tags
----
We just finished discussing Git’s three main object types (blobs, trees and commits),<br/>
but there is a fourth. The tag object is very much like a commit object — it contains a tagger, a date, a message, and a pointer.<br/>

Easy to update the lightweight tag ref<br/>
$ git update-ref refs/tags/v1.0 cac0cab538b970a37ea1e769cbbde608743bc96d<br/>

$cat .git/refs/remotes/git/git|remote branch 
                        |remote name
57cf1307c0eedb69bb5df7426bbe925957320d12

Git Internals - Packfiles
-------------------------
Packfiles
---------
It turns out that it can. The initial format in which Git saves objects on disk is called a “loose” object format.<br/>
However, occasionally Git packs up several of these objects into a single binary file called a “packfile” in order<br/>
to save space and be more efficient<br/>

Git Internals - The Refspec
---------------------------

The Refspec
-----------

$ git remote add jenkins https://github.com/Sadashiv/jenkins<br/>
Running the command above adds a section to your repository’s .git/config<br/>

$cat .git/config
[remote "jenkins"]
    url = https://github.com/Sadashiv/jenkins.git
    fetch = +refs/heads/*:refs/remotes/jenkins/*
we can add more fetch
    fetch = +refs/heads/master:refs/remotes/jenkins/master
    fetch = +refs/heads/git:refs/remotes/jenkins/git
    push = refs/heads/git:refs/remotes/jenkins/git

$ git fetch git git:refs/remotes/git/git
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/Sadashiv/git
   63b6597..60b4000  git        -> git/git

Pushing Refspecs
----------------
[remote "jenkins"]
    url = https://github.com/Sadashiv/jenkins.git
    fetch = +refs/heads/*:refs/remotes/jenkins/*
    fetch = +refs/heads/master:refs/remotes/jenkins/master
    fetch = +refs/heads/git:refs/remotes/jenkins/git
    push = refs/heads/git:refs/remotes/jenkins/git

Deleting References
-------------------
$ git push origin :topic

In git 1.7.0
$ git push origin --delete topic

Git Internals - Transfer Protocols
----------------------------------

1. Dumb Protocol
2. Smart Protocol

Git Internals - Maintenance and Data Recovery
---------------------------------------------

Maintenance and Data Recovery
-----------------------------

Occasionally, Git automatically runs a command called “auto gc”. Most of the time,<br/>

You can run auto gc manually as follows:<br/>

$ git gc --auto<br/>

If you run git gc, you’ll no longer have these files in the refs directory.<br/>
Git will move them for the sake of efficiency into a file named .git/packed-refs that looks like this:<br/>

$ cat .git/packed-refs
# pack-refs with: peeled fully-peeled sorted
63b659770e987e9d22a712996850c682aca8b614 refs/heads/git
63b659770e987e9d22a712996850c682aca8b614 refs/remotes/git/git
81de04d79d5245dbf790baf40c52135435e0f69a refs/tags/v1.1
^4b61022712899106be53b9dfdecf2a5241ab635e

Notice the last line of the file, which begins with a ^. This means the tag directly above is an annotated<br/>
tag and that line is the commit that the annotated tag points to.<br/>

Data Recovery
-------------
$cat .git/refs/heads/git<br/>
2b9f0585d478f2a7cce44bcc55e3c1efd8f0e758<br/>

git reset --hard commitid

The reflog is also updated by the git update-ref command, which is another reason to use it instead of just<br/>
writing the SHA-1 value to your ref files, as we covered in Git References. You can see where you’ve been at<br/>
any time by running git reflog:<br/>

$ git reflog
2b9f058 (HEAD -> git, git/git) HEAD@{0}: pull git git: Fast-forward<br/>
63b6597 HEAD@{1}: pull git git: Merge made by the 'recursive' strategy.<br/>
1540505 HEAD@{2}: commit: Update the git tips and tricks<br/>

$ git fsck --full<br/>
Checking object directories: 100% (256/256), done.<br/>
Checking objects: 100% (1272/1272), done.<br/>
dangling tree a0a738c5a8aa58d2b3907b84b93302f5dac220ba<br/>
dangling blob 83baae61804e65cc73a7201a7252750c76066a30<br/>

In this case, you can see your missing commit after the string “dangling commit”. You can recover it the same way,<br/>
by adding a branch that points to that SHA-1.<br/>

Removing Objects
----------------
There are a lot of great things about Git, but one feature that can cause issues is the fact that a git clone downloads<br/>
the entire history of the project, including every version of every file. This is fine if the whole thing is source code,<br/>
because Git is highly optimized to compress that data efficiently. However, if someone at any point in the history of your<br/>
project added a single huge file, every clone for all time will be forced to download that large file, even if it was removed<br/>
from the project in the very next commit. Because it’s reachable from the history, it will always be there.<br/>

Be warned: this technique is destructive to your commit history.
----------------------------------------------------------------
It rewrites every commit object since the earliest tree you have to modify to remove a large file reference. If you do this<br/>
immediately after an import, before anyone has started to base work on the commit, you’re fine – otherwise, you have to notify<br/>
all contributors that they must rebase their work onto your new commits.<br/>

$ git count-objects -v<br/>
count: 2<br/>
size: 8<br/>
in-pack: 1278<br/>
packs: 1<br/>
size-pack: 539<br/>
prune-packable: 0<br/>
garbage: 0<br/>
size-garbage: 0<br/>

ls .git/objects/pack/<br/>
pack-38ca2c173f154aa93a018d4bab87a4a9b484096a.idx pack-38ca2c173f154aa93a018d4bab87a4a9b484096a.pack<br/>

file ends with .pack contains the huge old and unwanted onjects<br/>

git verify-pack -v .git/objects/pack/pack-7b03cc896f31b2441f3a791ef760bd28495697e6.idx | sort -k 3 -n | tail -10<br/>

blob 6979 670 156343 7647cbbe10e7638d71079fd8f6ca295e89941a51<br/>
blob 10107 3726 68483 050c88e2b648e1f9622f56cdff28005b8724fadd<br/>
blob 12302 4462 186227 3b550a4b0111d8befa6272cd5d11ed8a7f545779<br/>
blob 13596 4808 171955 3280709c2f835e562b99a75180e858f450a429aa<br/>
blob 15167 6558 118383 ee8eb338da07df093cd2cfb4cb09337b6031bc36<br/>
blob 15335 6243 128430 24fa51b794dc3dce8982baf21343f09c66df1261<br/>
blob 16977 5795 160572 79e2d4ec85799442dd158343d351fd3c3be112af<br/>
blob 19201 5188 93224 e1b1eaf0e38faf452366216d61ff49895a5579ab<br/>
blob 35858 6332 134997 2e1b5e14b92fe61e02df94ab165c8aab6c06c9cb<br/>

git rev-list --objects --all | grep 2e1b5e14b92fe61e02df94ab165c8aab6c06c9cb<br/>
git filter-branch --index-filter<br/>
git rm --cached --ignore-unmatch 'all.pdf' --all<br/>
rm -Rf .git/refs/original<br/>
rm -Rf .git/logs/<br/>
git gc --aggressive --prune=now<br/>
or
git prune --expire now<br/>
gc(Garbage collect)<br/>

Git Internals - Environment Variables
-------------------------------------
$ git --exec-path<br/>
  /usr/lib/git-core<br/>

Repository Locations
--------------------
GIT_DIR -> .git/<br/>
GIT_OBJECT_DIRECTORY --> .git/objects<br/>

Pathspecs<br/>
Committing<br/>
Networking<br/>
GIT_ASKPASS is an override for the core.askpass configuration value.<br/>




