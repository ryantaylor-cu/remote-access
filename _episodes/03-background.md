---
title: "Jobs in the background"
teaching: 15
exercises: 0
questions:
- "Can I run a job without being connected to the server?"
- "Now that my job is running in the background, how do I control it?"
objectives:
- "First objective."
keypoints:
- "First key point."
---

## Running Processes in the Background

Sometimes if a job is going to take a long time to execute, we would like to run this process in the background. This allows us to have our job execute but still lets us have full control over our terminal.

To run a job in the background, simply add an & to the end of the command line before executing it. For example:

~~~
$ command &
~~~
{: .bash}

When we do this it will launch command and return us to the command prompt. If we run top, we should see it in our process list. 

If you have already started a job and want to relegate it to the background, press CTRL-Z. This will suspend the job. When you follow this by bg it will have it run in the background:

~~~
$ command
<CTRL-Z>
$ bg
~~~
{: .bash}



To get a list of jobs running in the background, we use the `jobs` command. Each background job is assigned a job ID. If we want to switch to a background job (ie. bring it to the foreground), we use the command `fg <ID>`. <ID> can be omitted if there is only one background process running.

If you want to end a process that is running, you can use the `kill <ID>` command. The kill command takes a process ID as a parameter. This ID can be determined using the top command.

## Redirecting Process Output to a File

Running certain programs cause a lot of information to be dumped to the console. Sometimes this is done so quickly that we cannot follow what is going on or we would like to save this output for later. To do this, we can use something called a redirect. This redirect causes the information normally dumped to your screen to be saved to a file. You do this by using ‘>’:
command > results.txt
This will save the output of command to results.txt. This is great, however there are actually two sets of outputs being produced here. The first is the regular output produced by command (known are stdout) as well as all of the errors produced (known as stderr). These “output streams” are normally dumped to the same location so the end user does not know where a given line came from. Sometimes it is useful to separate the stdout from the stderr. We can redirect either of these streams to a file by identifying them with ‘1’ (stdout) or ‘2’ (stderr). For example, if we want to save the stdout to a file and have the errors print to screen, we would do the following:

~~~
$ command 1> results.txt
~~~
{: .bash}

On the other hand, if we would like to print the stdout to screen and save the errors to a file, we would do the following:

~~~
$ command 2> errors.txt
~~~
{: .bash}

If we want to save both streams to separate files, we would do the following:

~~~
$ command 1> results.txt 2> errors.txt
~~~
{: .bash}

Finally, we can combine this with what we learned in the previous section to send this entire process to the background:

~~~
$ command 1> results.txt 2> errors.txt &
~~~
{: .bash}

## Using tmux to run an interactive shell in the background

As we have seen, you can run non-interactive jobs in the background.  We usually send the job's output to files.

Another option is to run a terminal shell in the background.  You can reconnect later to the background shell later.  The actual processes running in the background shell can be in the foreground.  You are even able to log out while the background shell session is running.

Using `tmux` you can run commands on the VM, logout, and reconnect to your running programs later. You can tell that you are running tmux by the green bar along the bottom of your Linux command-line window.


#### Creating a new session

This command will start a new tmux session.  You will see a new Linux command line that is running inside tmux.  Each tmux session has a name, so you can have different sessions, and reference them by their unique names.

~~~
$ tmux new -s example01
~~~
{: .bash}

#### Leaving (detaching) a session

This leaves a session, but it will keep running so you can resume the session later. When you are in your tmux session, type:

~~~
$ Ctrl-b followed by d
~~~
{: .bash}

#### Listing sessions

You can have more than one tmux session.  To see the list of running sessions, use this command:

~~~
$ tmux ls
~~~
{: .bash}

#### Resuming (attaching) a session

When you want to reconnect to a session that you have already started, use this command:

~~~
$ tmux attach -t example01
~~~
{: .bash}

#### Stopping (killing) a session

Once you are finished with a tmux session, you can get rid of it using this:

~~~
$ tmux kill-session -t example01
~~~
{: .bash}

OR, type the Linux command exit inside your tmux session
