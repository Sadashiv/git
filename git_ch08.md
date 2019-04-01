 Customizing Git - Git Configuration
 ===================================

Most of configuration covered in chapter 1 and 2

Git Atrributes
--------------

gitattributes file in one of your directories (normally the root of your project)<br/>
or in the .git/info/attributes file if you don’t want the attributes file committ<br/>
ed with your project.<br/>

Inside .gitattributes
all.docx diff=word

https://sourceforge.net/projects/docx2txt<br/>
tar zxf docx2txt-1.4.tgz<br/>
git config diff.word.textconv docx2txt<br/>

Convert the doc2txt<br/>
./docx2txt.sh doc/gitattributes.docx<br/>

git diff<br/>

Customizing Git - Git Hooks
---------------------------
1. Client-side hooks<br/>
2. Server-side hooks<br/>

$ ls .git/hooks
applypatch-msg.sample      pre-applypatch.sample      pre-rebase.sample
commit-msg.sample          pre-commit.sample          pre-receive.sample
fsmonitor-watchman.sample  prepare-commit-msg.sample  update.sample
post-update.sample         pre-push.sample


$ cp .git/hooks/pre-commit.sample .git/hooks/pre-commit<br/>

$ chmod +x .git/hooks/pre-commit<br/>

$ .git/hooks/pre-commit<br/>

Customizing Git - An Example Git-Enforced Policy
-------------------------------------------------

Server-Side Hook<br/>
All the server-side work will go into the update file in your<br/>
hooks directory. The update hook runs once per branch being<br/>
pushed and takes three arguments:<br/>

--> The name of the reference being pushed to<br/>
--> The old revision where that branch was<br/>
--> The new revision being pushed<br/>

Enforcing a User-Based ACL System
---------------------------------
Suppose you want to add a mechanism that uses an access control<br/>
list (ACL) that specifies which users are allowed to push changes<br/>
to which parts of your projects. Some people have full access, and<br/>
others can only push changes to certain subdirectories or specific<br/>
files. To enforce this, you’ll write those rules to a file named acl<br/>
that lives in your bare Git repository on the server. You’ll have the<br/>
update hook look at those rules, see what files are being introduced<br/>
for all the commits being pushed, and determine whether the user doing<br/>
the push has access to update all those files.<br/>
