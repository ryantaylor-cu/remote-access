---
title: "Remote GUI programs"
teaching: 10
exercises: 0
questions:
- "How can I run a program on a remote computer but still use the graphical user interface that I'm used to?"
objectives:
- "Setup an ssh connection to allow remote GUI programs"
- "Run a GUI program"
keypoints:
- "The ssh `-X` option allows us to run remote GUI programs"
---


We have focussed on using commands in the Linux shell to work on the remote server.  It is also possible to run GUI (graphical user interface) software programs on the remote server.  This has benefits and drawbacks.  It can be easier to work with the familiar graphical interface to an application.  However, over a slow network connection graphical applications might not be very responsive.

As an example, let's use a text editor called `gedit`.  Before runing the text editor, we need to reconnect to the server using the `-X` option.  The capital X tells ssh to allow remote GUI applications:

~~~
server$ logout
local$ ssh -X jane@192.0.2.5
server$ gedit &
~~~

> ## When in doubt, use ssh -X 
>
> When connecting to a remote server on which you would like to run programs with GUIs, it is best to use the -X option as this provides the most secure connection. However, security settings on different computers vary and it is possible that the remote machine will not be trusted to forward the program's GUI over the ssh connection. If this is the case, we can force our computer to trust the remote machine by replacing the -X option with the -Y option. If you find that -X doesn't work then do the following:
>
> ~~~
> ssh -Y jane@192.0.2.5
> ~~~
{: .callout}

## Examples of other graphical applications

For your reference, here are a few other GUI applications that you might have access to at Carleton University:

### SPSS

~~~
statistics &
~~~
{: .bash}

### STATA

~~~
xstata &
~~~
{: .bash}

### Matlab

~~~
matlab &
~~~
{: .bash}
