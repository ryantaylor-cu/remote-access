---
title: "Interactive shell in the background"
teaching: 15
exercises: 0
questions:
- "How do I run jobs interactively in the shell, but also logout and reconnect?"

objectives:
- "First objective."
keypoints:
- "First key point."
---

## Using tmux to run an interactive shell in the background

As we have seen, you can run non-interactive jobs in the background.  We usually send the job's output to files.

Another option is to run an entire shell session in the background using a special program called `tmux`.  This way, you can reconnect later to the background shell later and jobs will still be running, even an interactive foreground job.  You are even able to log out while the background shell session is running.

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
