Git and Other Systems - Git as a Client
=======================================

1. Install git-svn<br/>
$ sudo apt-get install git-svn<br/>
$ sudo yum install git-svn<br/>

mkdir svn-test
svnadmin create svn-test/
git svn clone svn-test/ svngitrepo

git and mercurial(hg)
---------------------
The DVCS universe is larger than just Git. In fact, there are many other systems in<br/>
this space, each with their own angle on how to do distributed version control correctly.<br/>
Apart from Git, the most popular is Mercurial, and the two are very similar in many respects.<br/>

git-p4
------
Git-p4 is a two-way bridge between Git and Perforce. It runs entirely inside your Git<br/>
repository, so you won’t need any kind of access to the Perforce server (other than <br/>
user credentials, of course). Git-p4 isn’t as flexible or complete a solution as Git<br/>
Fusion, but it does allow you to do most of what you’d want to do without being invasive<br/>
to the server environment.<br/>


Git and TFS
-----------
Git is becoming popular with Windows developers, and if you’re writing code on Windows,<br/>
there’s a good chance you’re using Microsoft’s Team Foundation Server (TFS)<br/>

Git and Other Systems - Migrating to Git
----------------------------------------

$ git svn clone http://my-project.googlecode.com/svn/ \
  --authors-file=users.txt --no-metadata --prefix "" -s my_project<br/>
$ cd my_project<br/>
$ git svn log<br/>

To move the tags to be proper Git tags, run:<br/>

$ for t in $(git for-each-ref --format='%(refname:short)'\ <br/>
  refs/remotes/tags); do git tag ${t/tags\//} $t && git branch -D -r $t; done<br/>

Mercurial
---------

$ git clone https://github.com/frej/fast-export.git<br/>
The first step in the conversion is to get a full clone of the Mercurial repository you want to convert:<br/>

$ hg clone <remote repo URL> /tmp/hg-repo<br/>

