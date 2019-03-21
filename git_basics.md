Git Basics
==========

Creating the new repo in local
mkdir jenkins
cd jenkins
git init
Initialized empty Git repository in /home/sadashiv/jenkins/.git/

$ git add README.md
$ git commit README.md -m "Initial commit"

Cloning the existing

https requires Auth: https://github.com/Sadashiv/jenkins
ssh: git@github.com:Sadashiv/jenkins

Image

git status

vim run.sh
#!/bin/bash
java -jar jenkins.war --daemon

Tracking the file
git add run.sh

Staging hte modified file
git add README.md

git status

ignoreing the files
vim .git/gitignore
__init__.pyc

git diff 
git diff --staged
git diff --cached
#Cached and staged are synonyms


You can skip the staged files
remove files
git rm README.md
git rm --cached README.md

moving files
git mv run.sh start_jenkins.sh

viewing the history
git log
git log [ADOG] --all --decorate --oneline --graph

Undoing the things
git commit --amend

Save the working directory
git stash
git stash apply

Revert modified content 
git checkout -- README.md
git remote -v

buildout	https://github.com/Sadashiv/buildout (fetch)
buildout	https://github.com/Sadashiv/buildout (push)
dotfiles	https://github.com/Sadashiv/dotfiles (fetch)
dotfiles	https://github.com/Sadashiv/dotfiles (push)
git	https://github.com/Sadashiv/git (fetch)
git	https://github.com/Sadashiv/git (push)
jenkins	https://github.com/Sadashiv/jenkins.git (fetch)
jenkins	https://github.com/Sadashiv/jenkins.git (push)
origin	https://github.com/Sadashiv/gittraining.git (fetch)
origin	https://github.com/Sadashiv/gittraining.git (push)

git remote add <remotename> <remote_https_ssh_url>

git fetch origin

Inspecting a Remote
git remote show origin
git remote rename <old_remote> <new_remote>
git remote rm <remotename>

Git tagging
Three types of tagging
 I. Plaini/Lightweight tagging
    git tag <tagname>
    git tag v0.1 - This will not show who created the tag

 II. Annoted tagging
     git tag -a <tagname> -m "Message for tag"

 III. Signed tag are cumbersome and safe to pass the tags with sigining key
      git tag -s <tagname> -m "Message for tag"

Tag @particular commit hash-> git tag -a v1.2 9fceb02
Push tag
git push origin <tagname>
git push origin --tags #Push all the tags

Delete local tag -> git tag -d <tagname>
Delete Remote tag -> git push origin :<tagname> or git push origin --delete <tagname>
Checking out tag
git checkout <tagname>
or
git checkout -b <branchname> <tagname>

Set git aliases globally or Downoad the files
https://github.com/Sadashiv/dotfiles
------------------------------------
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status

Soft revert of file
git reset HEAD -- README.md
file will be avail in local
git reset --hard README.md 
Changes will be gone away permnently

