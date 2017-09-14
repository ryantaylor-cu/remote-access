---
title: "File Permissions"
teaching: 30
exercises: 0
questions:
- How do I control access to my data?
objectives:
- Understand ownership and access permissions on files and directories
keypoints:
- The `chown` command controls file user and group ownership
- The `chmod` command controls file permissions
- Permissions exist for read, write, and execute
- Permission apply to user, group, and other
---

## Users and Groups

We will be talking about permissions on Linux files and directories.  These permissions allow you to decide who can access your data.  Perhaps you want to have all your files restricted so only you can access them. Or you can choose to share certain files and directories with your colleagues.

We have already seen how to connect to a remote Linux computer using ssh.  Each of us has our own username and password to do so - Linux computers have more than one user account.  One set of permissions is affiliated with a file's user.

Users are also collected into groups.  All users are in one primary group and can optionally be in other groups as well.  Some Linux systems have a primary group per user.  Other systems may have one primary group shared for all users.  There is another set of permissions affiliated with a file's group.

## Performing Administrative Tasks

When we talk about users and groups, we are starting to touch upon system administration.  In other words, we are configuring the computer's settings for all users rather than just working with our own user's account and files.

Linux has a special user called `root`.  The root user can do anything to the operating system and so root is used to modify computer settings. On many types of Linux, you can run configuration commands as root using the `sudo` command.  If you put `sudo` at the start of a shell command, that command will run as the administrative `root` user.

> ## Dangers of `sudo`
> It is a good idea to avoid `sudo` unless you are already accustomed to using it.  Commands run with `sudo` could cause problems not just for your own user account and files, but for everybody on the Linux system.
{: .callout }

You may recall that the `whoami` command tells you your username:

~~~
$ whoami
~~~
{: .bash }
~~~
jane
~~~
{: .output }


If we use `sudo` to run this same command, then we do see that the `sudo` runs our `whoami` commands as the root user:

~~~
$ sudo whoami
~~~
{: .bash }
~~~
root
~~~
{: .output }

## Setting ownership

There are various common tasks system administrators perform using `sudo`, such as software installation, user account creation, and setup hardware.  Changing the owner of a file must also be done using sudo.

Every file and directory in Linux is owned by a user and by a group.  The `ls -lh` command's listing shows the user and group owning a file in the third and fourth columns respectively:

~~~
$ ls -lh data.zip
~~~
{: .bash }
~~~
-rw-rw-r-- 1 jane jane 5.2K Aug 31 11:20 data.zip
~~~
{: .output }

To change the user and group of a file, one needs to use the chown command.  The user and then group are separated by a colon:

~~~
$ sudo chown root:root data.zip
# ls -lh data.zip
~~~
{: .bash }
~~~
-rw-rw-r-- 1 root root 5.2K Aug 31 11:20 data.zip
~~~
{: .output }


## File Permissions

Let's look at the long format output of `ls` again.  This time, we will look at the codes at the start of each output line.  The letters you see specify who is allowed to use the file. These letters might be a little bit hard to understand at first, but we will explain their meaning in detail:

~~~
$ ls -lh
~~~
{: .bash }
~~~
total 28K
-rw-rw-r-- 1 jane jane 5.2K Aug 31 11:20 data.zip
drwxrwxr-x 2 jane jane 4.0K Sep  5 11:03 Desktop
-rwx------ 1 jane jane  14K Aug 30 15:52 run_simulation
~~~
{: .output }

Permissions for a file are listed at the start of `ls` in 10 characters.  The first character actually doesn't have to do with permissions, but rather indicates the file type; `-` represents a file and `d` represents a directory.

![File Permissions](../fig/permissions.svg)

The file permissions are `rwx`, where:
 - The **r**ead permission on a file allows you to view the data in the file
 - The **w**rite permission on a file allows you to modify or delete the file
 - The e**x**ecute permission on a file allows you to run the file (assumes the file is a program or shell script)

The following 9 characters are grouped into `rwx` permissions for each of the three user roles:
 - The **u**ser is the person who owns the file, usually the file's creator
 - The **g**roup is the group of users associated with the file
 - Finally **o**ther specifies anybody else who isn't the file's user or in the file's group

In the output of `ls`, when a permission is shown as `-`, the particular file does not have that particular permission.

For example, in the preceding `ls -lh` output we see that `data.zip` has permissions `-rw-rw-r--`.  The first `-` means that we are looking at a file.  The next `rw-` means that the user `jane` can view or modify this file, but not execute it.  This makes sense, since it is Jane's file, and it is not a program that could be run.  Similarly, anybody in the group `jane` can read or modify the file.  All other users on this Linux computer could read but not change the file due to the `r--` permissions for other.

## Directory Permissions

Directories have the same three permissions: read, write, and execute.  However, permissions have different meanings for directories.

The directory permissions are `rwx`, where:
 - The **r**ead permission on a directory allows you to run `ls` to list the files in that directory
 - The **w**rite permission on a directory allows you to create or delete files in the directory
 - The e**x**ecute permission on a file allows you get to the files and directories inside.

Let's look at the `ls` output again:

~~~
$ ls -lh
~~~
{: .bash }
~~~
total 28K
-rw-rw-r-- 1 jane jane        5.2K Aug 31 11:20 data.zip
drwxr-x--x 2 jane researchers 4.0K Sep  5 11:03 Desktop
-rwx------ 1 jane jane         14K Aug 30 15:52 run_simulation
~~~
{: .output }

In this output, we see that Desktop is a directory.  We know this because the first letter in the permissions entry is `d`.  The user `jane` has full access to `Desktop`.  Users in the `researchers` group can see what files are in `Desktop` and are able to use them.  For other users, the files in `Desktop` are hidden, although the files still can be used.

## Changing Permissions

The command to change permissions is called `chmod`.  The permissions are modified with a special three character code.

The first letter in the code is either **u**, **g**, or **o** and specifies whether we are modifying the permission for **u**ser, **g**roup, or **o**ther role.

The second letter is either **+** or **-**, and indicates whether we are adding or removing a permission.

The third letter either **r**, **w**, **x** and indicates wheter we are modifying the **r**ead, **w**rite, or e**x**ecute permission.

The most common use of `chmod` would be to add the execute permission to a program that you have downloaded.  For example, let's remove the ability to execute our run_simulation program:

~~~
chmod u-x run_simulation
./run_simulation
~~~
{:.bash }
~~~
bash: ./mycommands: Permission denied
~~~
{: .output }

The `u+x` will add allow the user to execute this file:

~~~
chmod u+x run_simulation
./run_simulation
~~~
{: .bash }

> ## Fixing permissions
>
> Your supervisor has given you a new program to
>run, called sim2.  You can find it in the data.zip file.
>Unfortunately, your supervisor accidentally changed permissions on
>the sim2 program and now it won't run.
>
> ~~~
> $ ./sim2
> ~~~
> {: .bash }
> ~~~
> -bash: ./sim2: Permission denied
> ~~~
> {: .output }
>
> Using your new knowledge of Linux file permissions, how would you
>determine the problem and what command would correct the permission
>error on the sim2 program?
>
> > ## Solution
> > 
> > To see the current permissions on sim2:
> > ~~~
> > ls -lh sim2
> > ~~~
> > {: .bash }
> > ~~~
> > ----r----- 1 jane researchers 8.3K Sep 13 10:28 sim2
> > ~~~
> > {: .output }
> >
> > And to allow your own user to run the program:
> > ~~~
> > chmod u+x sim2
> > ~~~
> > {: .bash }
> {: .solution}
{: .challenge}

> ## Restricting Access
> The `ls` command lists your file's access like this:
> ~~~
> -rw-r--r-- 1 jane researchers  12K Sep 13 10:33 confidential.txt
> ~~~
> {: .output }
> `chmod` commands would change this file's permissions so only your
> user can view the file?
>
> > ## Solution
> > ~~~
> > chmod g-r confidential.txt
> > chmod o-r confidential.txt
> > ~~~
> {: .solution }
{: .challenge }

> ## Reading Permissions
> Jane has a directory somestuff that contains a program mysim:
> ~~~
> ls -l
> ~~~
> {: .bash }
> ~~~
> drwxr----x 2 jane researchers 4.0K Sep  5 09:22 somestuff
> ~~~
> {: .output }
>
> ~~~
> ls -l somestuff/
> ~~~
> {: .bash }
> ~~~
> -rw-r-xr-x 1 jane researchers 8.3K Sep 13 10:32 mysim
> ~~~
> {: .output }
>
> What can can these users do with the mysim program?
>  1. The user `jane`?
>  1. The user `bob`, who is in the `researchers` group?
>  1. The user `alice`, who is not in the `researchers` group?
>
>> ## Solution
>> 1. Jane can see the program with ls and change the mysim, but she cannot run it.
>> 1. Bob cannot use `ls` to see that the program exists, but he can run the program `somestuff/mysim`.
>> 1. Alice can see the program exists by using `ls`, but she cannot run the program.
> {: .solution }
{: .challenge }
