Git Internals - Plumbing and Porcelain
======================================

Plumbing and Porcelain
----------------------
As you will have noticed by now, this book’s first nine chapters deal almost<br/>
exclusively with porcelain commands. But in this chapter, you’ll be dealing<br/>
mostly with the lower-level plumbing commands, because they give you access<br/>
to the inner workings of Git,<br/>

mkdir bare<br/>
cd bare/<br/>
git init --bare<br/>

bare (BARE:master)]$ ls -F1<br/>
branches/<br/>
config<br/>
description<br/>
HEAD<br/>
hooks/<br/>
info/<br/>
objects/<br/>
refs/<br/>

This leaves four important entries: the HEAD and (yet to be created) index<br/>
files, and the objects and refs directories. These are the core parts of Git.<br/>
The objects directory stores all the content for your database, the refs<br/>
directory stores pointers into commit objects in that data<br/>
(branches, tags, remotes and more), the HEAD file points to the branch<br/>
you currently have checked out, and the index file is where Git stores<br/>
your staging area information. You’ll now look at each of these sections<br/>
in detail to see how Git operates.<br/>

Git Internals - Git Objects
---------------------------

Git is a content-addressable filesystem. Great. What does that mean?<br/>
It means that at the core of Git is a simple key-value data store.<br/>
What this means is that you can insert any kind of content into a<br/>
Git repository, for which Git will hand you back a unique key you<br/>
can use later to retrieve that content.

Objects directory
-----------------
$ find .git/objects -type f<br/>
.git/objects/81/3305db086b547ddb82d790d6ad78d1543568fe<br/>
.git/objects/a9/4f7100df1643f721b1a02dfb6ddc570c85a908<br/>


Now, you can add content to Git and pull it back out again. You can<br/>
also do this with content in files. For example, you can do some<br/>
simple version control on a file. First, create a new file and save<br/>
its contents in your database:<br/>

$ echo 'version 1' > test.txt<br/>
$ git hash-object -w test.txt<br/>
83baae61804e65cc73a7201a7252750c76066a30<br/>
Then, write some new content to the file, and save it again:<br/>

$ echo 'version 2' > test.txt<br/>
$ git hash-object -w test.txt<br/>
1f7a7a472abf3dd9643fd615f6da379c4acb3e3a<br/>
Your object database now contains both versions of this new file<br/>
(as well as the first content you stored there):<br/>

$ find .git/objects -type f<br/>
.git/objects/1f/7a7a472abf3dd9643fd615f6da379c4acb3e3a<br/>
.git/objects/83/baae61804e65cc73a7201a7252750c76066a30<br/>
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4<br/>
At this point, you can delete your local copy of that test.txt<br/>
file, then use Git to retrieve, from the object database, either<br/>
the first version you saved:<br/>

$ git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30 > test.txt<br/>
$ cat test.txt<br/>
version 1<br/>
or the second version:<br/>

$ git cat-file -p 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a > test.txt<br/>
$ cat test.txt<br/>
version 2<br/>
But remembering the SHA-1 key for each version of your file isn’t<br/>
practical; plus, you aren’t storing the filename in your system —<br/>
just the content. This object type is called a blob. You can have<br/>
Git tell you the object type of any object in Git, given its SHA-1<br/>
key, with<br/>

$ git cat-file -t:<br/>

$ git cat-file -t 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a<br/>
blob<br/>


Tree objects
------------
|Windows: git cat-file -p master^^{tree}|<br/>

$ git cat-file -p git^{tree}<br/>
100644 blob 09eafcdb2ace716f9cfbb2454cba81f8c9c16e89	README.md<br/>
100644 blob 32a6d4345b2737fa9512a309ecbe9a70ed16513e	git_ch01.md<br/>
040000 tree 3dd40c3014d704d141846704ed3276f2c73aa20b	images<br/>
160000 commit 69f4cfa2010babaab1284f79931f8a569c5667f8	jenkins<br/>

In this case, you’re specifying a mode of 100644, which means<br/>
it’s a normal file. Other options are 100755, which means it’s<br/>
an executable file; and 120000, which specifies a symbolic link.<br/>
The mode is taken from normal UNIX modes but is much less flexible<br/>
— these three modes are the only ones that are valid for files<br/>
(blobs) in Git (although other modes are used for directories<br/>
and submodules).<br/>

Conceptually, the data that Git is storing looks something like this:
---------------------------------------------------------------------
![Tree objects](images/data-model-1.png)

$ git write-tree<br/>
6b52fa8f97f65c132abf042c7ebd48896abc69b1<br/>
$ git show 6b52fa8f97f65c132abf042c7ebd48896abc69b1<br/>
tree 6b52fa8f97f65c132abf042c7ebd48896abc69b1<br/>

.gitmodules<br/>
README.md<br/>
git.txt<br/>
git_ch01.md<br/>
git_ch02.md<br/>
git_ch03.md<br/>
git_ch04.md<br/>
git_ch05.md<br/>
git_ch06.md<br/>
git_ch07_01.md<br/>
git_ch07_02.md<br/>
git_ch08.md<br/>
git_ch09.md<br/>
images/<br/>
jenkins<br/>

These three main Git objects — the blob, the tree, and the commit<br>
— are initially stored as separate files in your .git/objects<br/>
directory. Here are all the objects in the example directory now,<br/>
commented with what they store:<br/>

$ find .git/objects -type f<br/>

Object Storage
--------------
We mentioned earlier that there is a header stored with every object<br/>
you commit to your Git object database. Let’s take a minute to see<br/>
how Git stores its objects. You’ll see how to store a blob object —<br/>
 in this case, the string “what is up, doc?” — interactively in the<br/>
Ruby scripting language.<br/>

You can start up interactive Ruby mode with the irb command:<br/>

$ irb<br/>
>> content = "what is up, doc?"<br/>
=> "what is up, doc?"<br/>
Git first constructs a header which starts by identifying the type<br/>
of object — in this case, a blob. To that first part of the header,<br/>
Git adds a space followed by the size in bytes of the content, and<br/>
adding a final null byte:<br/>

>> header = "blob #{content.length}\0"<br/>
=> "blob 16\u0000"<br/>
Git concatenates the header and the original content and then<br/>
calculates the SHA-1 checksum of that new content. You can<br/>
calculate the SHA-1 value of a string in Ruby by including the<br/>
SHA1 digest library with the require command and then calling<br/>
Digest::SHA1.hexdigest() with the string:

>> store = header + content<br/>
=> "blob 16\u0000what is up, doc?"<br/>
>> require 'digest/sha1'<br/>
=> true<br/>
>> sha1 = Digest::SHA1.hexdigest(store)<br/>
=> "bd9dbf5aae1a3862dd1526723246b20206e5fc37"<br/>
Let’s compare that to the output of git hash-object.<br/>
Here we use echo -n to prevent adding a newline to the input.<br/>

$ echo -n "what is up, doc?" | git hash-object --stdin<br/>
bd9dbf5aae1a3862dd1526723246b20206e5fc37<br/>
Git compresses the new content with zlib, which you can do in Ruby<br/>
with the zlib library. First, you need to require the library and<br/>
then run Zlib::Deflate.deflate() on the content:<br/>

>> require 'zlib'<br/>
=> true<br/>
>> zlib_content = Zlib::Deflate.deflate(store)<br/>
=> "x\x9CK\xCA\xC9OR04c(\xCFH,Q\xC8,V(-\xD0QH\xC9O\xB6\a\x00_\x1C\a\x9D"<br/>
Finally, you’ll write your zlib-deflated content to an object on disk.<br/>
You’ll determine the path of the object you want to write out (the first<br/>
two characters of the SHA-1 value being the subdirectory name, and the<br/>
last 38 characters being the filename within that directory). In Ruby,<br/>
you can use the FileUtils.mkdir_p() function to create the subdirectory<br/>
if it doesn’t exist. Then, open the file with File.open() and write out<br/>
the previously zlib-compressed content to the file with a write() call<br/>
on the resulting file handle:<br/>

>> path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]<br/>
=> ".git/objects/bd/9dbf5aae1a3862dd1526723246b20206e5fc37"<br/>
>> require 'fileutils'<br/>
=> true<br/>
>> FileUtils.mkdir_p(File.dirname(path))<br/>
=> ".git/objects/bd"<br/>
>> File.open(path, 'w') { |f| f.write zlib_content }<br/>
=> 32<br/>
Let’s check the content of the object using git cat-file:<br/>

$ git cat-file -p bd9dbf5aae1a3862dd1526723246b20206e5fc37<br/>
what is up, doc?<br/>

That’s it – you’ve created a valid Git blob object.<br/>

All Git objects are stored the same way, just with different<br/>
types – instead of the string blob, the header will begin with<br/>
commit or tree. Also, although the blob content can be nearly<br/>
anything, the commit and tree content are very specifically formatted.<br/>
