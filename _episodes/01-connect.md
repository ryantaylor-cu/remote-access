---
title: "Interacting with a server"
teaching: 30
exercises: 0
questions:
- "How to I get started with a Linux server?"
objectives:
- "Connect to a server"
keypoints:
- "ssh connects to a terminal session on the server."
- "scp copies files to and from the server."
---

##Connecting to and Disconnecting from a Remote Computer

In this lesson we will show you how to connect to a remote computer and transfer data you would like to work on to this remote machine. To start, you should have a newly opened shell window on your computer. Currently you can use this terminal to interact with your computer. Since we want to work on a remote computer, we need to connect our sheel to that computer. To do this, we will use a tool called ssh (secure shell). This is a standard protocol used to securely connect to computers over a network. Generally speaking, you would use ssh in the following manner:

~~~
ssh <remote-user-name>@<remote-computer-IP>
~~~

If you are using Carleton's RCDC, both of these pieces of information, along with your temporary password, will be provided. When you execute the ssh command and provide a username and IP address you will be prompted to add your password. In any context, if you are provided a temporary password (espcially if it was communicated to you over email), you should change it as soon as you can. Once you have successfully connected to the remote machine, you will likely be presented with some text describing the computer and operating system. After this you should then be left with a command prompt, just as you had before connecting. TO change your password, enter the following command:

~~~
passwd
~~~ 

You will then be asked to enter your current password followed by your new password twice. If you followed the instructions correctly, your password will be changed.


OK, so now we know how to connect, but how do we then disconnect?. TO do this, enter the following command:

~~~
exit
~~~

This will close the ssh connection and return your terminal to your own computer.

##Copying Files to and from a Remote Computer

Now that we know how to connect and disconnect from a remote computer, it is very likely that you would want to transfer files to the remote computer to process and then copy the results back to your computer. To do this we will use a standard tool called scp (secure copy). This tool works a lot like the standard unix tool cp (copy), which we expect you have used before. To copy a file from your computer to a remote computer, your scp command would look as follows:

~~~
scp <local-path/local-file> <remote-user-name>@<remote-computer-IP>:<remote-path>
~~~ 

On the other hand, if you'd like to copy a file from the remote computer onto your computer, the scp command would resemble the following:

~~~
scp <remote-user-name>@<remote-computer-IP>:<remote-path/remote-file> <local-path>
~~~

Now let's copy our data to the remote machine so we can start working on it. Navigate to the folder containing the data.zip prepared for this lesson. We are going to now copy this to the remote machine as follows:

~~~
scp data.zip <remote-user-name>@<remote-computer-IP>:~
~~~

The ~ at the end of the command is short hand for our home directory.

