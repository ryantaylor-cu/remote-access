---
title: "Interacting with a server"
teaching: 15
exercises: 0
questions:
- "How do I get started with a Linux server?"
objectives:
- "Understand how to run commands on a server"
- "Explain how to copy files between a server and my computer"
keypoints:
- "ssh connects to a terminal session on the server."
- "scp copies files to and from the server."
---

## Connecting to and Disconnecting from a Remote Computer

In this lesson we will show you how to connect to a remote computer and transfer data you would like to work on to this remote machine. To start, you should have a newly opened shell window on your computer. Currently you can use this terminal to interact with your computer. Since we want to work on a remote computer, we need to connect to a shell on that computer. To do this, we will use a command called ssh (secure shell). This is a standard protocol used to securely connect to computers over a network. Generally speaking, you would use ssh in the following manner:

~~~
$ ssh <remote-user-name>@<remote-computer-IP>
~~~
{: .bash}

> ## Are you connected?
> It can get confusing when you are switching between your own computer's shell and remote servers' shells.  All computers have a name that can be shown using the `hostname` command.  This lets you know what computer you are working on:
> ~~~
> $ hostname
> ~~~
> {: .bash}
{: .callout}

If you are using Carleton's RCDC, both of these pieces of information, along with your temporary password, will be provided. When you execute the ssh command and provide a username and IP address you will be prompted to add your password. In any context, if you are provided a temporary password (espcially if it was communicated to you over email), you should change it as soon as you can. Once you have successfully connected to the remote machine, you will likely be presented with some text describing the computer and operating system. After this you should then be left with a command prompt, just as you had before connecting. To change your password, enter the following command:

~~~
$ passwd
~~~ 
{: .bash}

~~~
Changing password for jane.
(current) UNIX password: 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
~~~
{: .output}

You will then be asked to enter your current password followed by your new password twice. If you followed the instructions correctly, your password will be changed.


OK, so now we know how to connect, but how do we then disconnect?. To do this, enter the following command:

~~~
$ exit
~~~
{: .bash}

This will close the ssh connection and return your terminal to your own computer.

## Copying Files to and from a Remote Computer

Now that we know how to connect and disconnect from a remote computer, it is very likely that you would want to transfer files to the remote computer to process and then copy the results back to your computer. To do this we will use a standard command called scp (secure copy). This command works a lot like the standard unix command cp (copy), which we expect you have used before. To copy a file from your computer to a remote computer, your scp command would look as follows:

~~~
$ scp <local-path/local-file> <remote-user-name>@<remote-computer-IP>:<remote-path>
~~~ 
{: .bash}

On the other hand, if you'd like to copy a file from the remote computer onto your computer, the scp command would resemble the following:

~~~
$ scp <remote-user-name>@<remote-computer-IP>:<remote-path/remote-file> <local-path>
~~~
{: .bash}

You can also copy entire directories, including all their contents, by adding the `-r` flag:
~~~
$ scp <remote-user-name>@<remote-computer-IP>:<remote-path> <local-path>
~~~
{: .bash}

Now let's copy our data to the remote machine so we can start working on it. Navigate to the folder containing the data.zip prepared for this lesson. We are going to now copy this to the remote machine as follows:

~~~
$ scp data.zip <remote-user-name>@<remote-computer-IP>:~
~~~
{: .bash}

The ~ at the end of the command is short hand for our home directory.

> ## Connect to a server
> If Jane wants to connect to her favourite RCDC server with address 192.0.2.26, which command should she use?
> 1. `ssh jane 192.0.2.26`
> 2. `scp jane@192.0.2.26:~ .`
> 3. `ssh jane@192.0.2.26`
> 4. `hostname 192.0.2.26`
> 5. `ssh jane:192.0.2.26`
> 
> > ## Solution
> > The correct answer is 3. The `ssh` command will connect Jane to a shell on a remote server, and the `@` sign separates her name from the computer address.
> {: .solution}
{: .challenge}

> ## Finding where you are
> You think you have used ssh to connect to a remote server, but you aren't sure.  Which of the following commands will show you what server you are connected to?
> 1. `hostname`
> 2. `whoami`
> 3. `pwd`
> 4. `computername`
> 
> > ## Solution
> > 1. Yes: `hostname` will show the name of the computer you are currently interacting with.
> > 2. No: `whoami` will show you the name of your user account
> > 3. No: `pwd` will show you current directory path
> > 4. No: `computername` is not a valid bash command
> {: .solution}
{: .challenge}

> ## Copying Data
> Assuming you do have access to remoteserver as user charlie, what is the error in this scp command?
> ~~~
> $ mkdir mydirectory
> $ scp mydirectory charlie@remoteserver:~
> ~~~
> {: .bash}
> 
> > ## Solution
> > If you run the above command, then you would get an error like this:
> > ~~~
> > mydirectory: not a regular file
> > ~~~
> > {: .error}
> > To copy a directory, you must use the `-r` option:
> > ~~~
> > $ scp -r mydirectory localhost:~
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
