Version control Systems
=======================

Git<br/>
CVS<br/>
SVN<br/>
Rational Rose<br/>
ClearCase<br/>
Mercurial<br/>
Bazaar<br/>


Centralized verson control
--------------------------
CVS, SVN<br/>
![Centralized Version Control Systems](images/centralized.png)

Distributes version control
---------------------------
Git<br/>
![Distributed Version Control Systems](images/distributed.png)

New system fallows few more feature
-----------------------------------
Speed<br/>
Simple design<br/>
Strong support for non-linear development (thousands of parallel branches)<br/>
Fully distributed<br/>
Able to handle large projects like the Linux kernel efficiently (speed and data size)<br/>

Git has Three states
--------------------
![Image description](working_copy.png)


Instaling git in Linux
----------------------
https://git-scm.com/download/linux<br/>
yum install git<br/>
apt install git<br/>

Install git in Windows
----------------------
Download git-version.exe and install<br/>
https://git-scm.com/download/win<br/>

Git config settings three ways
-------------------------------

System level -> /etc/gitconfig
-------------------------------
git config --system user.name "Sadashiv H"<br/>
git config --system user.email "sadashivhb@gmail.com"<br/>

User level -> /home/user/.gitconfig
-----------------------------------
git config --global user.name "Sadashiv H"<br/>
git config --global user.email "sadashivhb@gmail.com"<br/>

 repo level -> repo/.git/config
-------------------------------
git config user.name "Sadashiv H"<br/>
git config user.email "sadashivhb@gmail.com"<br/>

check your git settings
-----------------------
git config --list<br/>
git config <key><br/>
git config user.name<br/>

Git command help
----------------
git help config or git config --help<br/>
