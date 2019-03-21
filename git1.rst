Git-1
=====

# Version control Systems.

Git
CVS
SVN
Rational Rose
ClearCase
Mercurial
Bazaar


# Centralized verson control
CVS, SVN


# Distributes version control
Git

New system fallows few more feature
-----------------------------------
| Speed
| Simple design
| Strong support for non-linear development (thousands of parallel branches)
| Fully distributed
| Able to handle large projects like the Linux kernel efficiently (speed and data size)

Git has Three states
--------------------

<img src="working_copy.png"  alt="Markdown Monster icon" style="float: left; margin-right: 10px;" />
![Screenshot](working_copy.png)
![picture](img/working_copy)
.. image:: working_copy.png
    :width: 400px
    :align: center
    :height: 100px
    :alt: alternate text

Instaling git in Linux
----------------------
https://git-scm.com/download/linux
yum install git|br|
apt install git

Install git in Windows
----------------------
Download git-version.exe and install
https://git-scm.com/download/win

Git config settings three ways
-------------------------------

System level -> /etc/gitconfig
-------------------------------
git config --system user.name "Sadashiv H"
git config --system user.email "sadashivhb@gmail.com"

User level -> /home/user/.gitconfig
-----------------------------------
git config --global user.name "Sadashiv H"
git config --global user.email "sadashivhb@gmail.com"

repo level -> repo/.git/config
-------------------------------
git config user.name "Sadashiv H"
git config user.email "sadashivhb@gmail.com"

check your git settings
-------------------------------

git config --list

git config <key>
git config user.name

Git command help
-------------------------------
git help config or git config --help
