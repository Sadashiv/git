Git Basics
==========

Creating the new repo in local
------------------------------
mkdir jenkins<br/>
cd jenkins<br/>
git init<br/>
Initialized empty Git repository in /home/sadashiv/jenkins/.git/<br/>

$ git add README.md<br/>
$ git commit README.md -m "Initial commit"<br/>

Cloning the existing<br/>

https requires Auth:<br/>
git clone https://github.com/Sadashiv/jenkins<br/>
ssh:
git clone git@github.com:Sadashiv/jenkins<br/>

Image

git status<br/>

vim run.sh<br/>
#!/bin/bash<br/>
java -jar jenkins.war --daemon<br/>

Tracking the file
-----------------
git add run.sh<br/>

Staging hte modified file<br/>
git add README.md<br/>

git status<br/>

ignoreing the files<br/>
vim .git/gitignore<br/>
__init__.pyc<br/>

git diff<br/>
git diff --staged<br/>
git diff --cached<br/>
#Cached and staged are synonyms


You can skip the staged files<br/>
remove files<br/>
git rm README.md<br/>
git rm --cached README.md<br/>

moving files<br/>
git mv run.sh start_jenkins.sh<br/>

viewing the history<br/>
git log<br/>
git log [ADOG] --all --graph --oneline --decorate<br/>

Undoing the things<br/>
git commit --amend<br/>

Save the working directory<br/>
git stash<br/>
git stash apply<br/>

Revert modified content<br/>
git checkout -- README.md<br/>
git remote -v<br/>

buildout    https://github.com/Sadashiv/buildout (fetch)<br/>
buildout    https://github.com/Sadashiv/buildout (push)<br/>
dotfiles    https://github.com/Sadashiv/dotfiles (fetch)<br/>
dotfiles    https://github.com/Sadashiv/dotfiles (push)<br/>
git         https://github.com/Sadashiv/git (fetch)<br/>
git         https://github.com/Sadashiv/git (push)<br/>
jenkins     https://github.com/Sadashiv/jenkins.git (fetch)<br/>
jenkins     https://github.com/Sadashiv/jenkins.git (push)<br/>
origin      https://github.com/Sadashiv/gittraining.git (fetch)<br/>
origin      https://github.com/Sadashiv/gittraining.git (push)<br/>

git remote add <remotename> <remote_https_ssh_url><br/>

git fetch origin

Inspecting a Remote<br/>
git remote show origin<br/>
git remote rename <old_remote> <new_remote><br/>
git remote rm <remotename><br/>

Git tagging<br/>
Three types of tagging<br/>
 I. Plain/Lightweight tagging<br/>
    git tag <tagname><br/>
    git tag v0.1 - This will not show who created the tag<br/>

 II. Annoted tagging<br/>
     git tag -a <tagname> -m "Message for tag"

 III. Signed tag are cumbersome and safe to pass the tags with sigining key<br/>
      git tag -s <tagname> -m "Message for tag"<br/>

Tag @particular commit hash-> git tag -a v1.2 9fceb02
Push tag<br/>
git push origin <tagname><br/>
git push origin --tags #Push all the tags<br/>

Delete local tag -> git tag -d <tagname><br/>
Delete Remote tag -> git push origin :<tagname> or git push origin --delete <tagname><br/>
Checking out tag<br/>
git checkout <tagname><br/>
or<br/>
git checkout -b <branchname> <tagname><br/>

Set git aliases globally or Downoad the files<br/>
https://github.com/Sadashiv/dotfiles
------------------------------------
$ git config --global alias.co checkout<br/>
$ git config --global alias.br branch<br/>
$ git config --global alias.ci commit<br/>
$ git config --global alias.st status<br/>

Soft revert of file
-------------------
git reset HEAD -- README.md<br/>
file will be avail in local<br/>
git reset --hard README.md<br/>
Changes will be gone away permnently<br/>

![Image description](working_copy.png)
