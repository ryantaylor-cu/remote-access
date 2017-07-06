---
title: "Queuing many jobs with task spooler"
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

## Overview

Task Spooler (`tsp`) is not a standard Linux command.  However, we mention it here because if it happens to be available on your Linux server, it is a straightforward way to run many jobs.

The `tsp` command will a job in the background;  this lets you run other commans while `tsp` is busy running your program.  The `tsp` command will even keep running your jobs after you are logout or disconnect from the server.

Only one job will run with `tsp` at a time.  However, you can ask `tsp` to run more jobs.  Once the first job finishes, `tsp` will run the next job.  This is called *queuing*, and is analogous to the lineup at a grocery store.

## Listing tsp jobs

~~~
$ tsp
~~~
{. :bash}
~~~
ID   State      Output               E-Level  Times(r/u/s)   Command [run=1/1]
1    running    /tmp/ts-out.IiCgZe                           sleep 10
0    finished   /tmp/ts-out.QUFZ40   0        2.00/0.00/0.00 sleep 2
~~~
{. :output}

## Running jobs with tsp

Just put `tsp` in front of the command you want to run.  Note that `tsp` can only directly run a single command, but does not directly handle the special features of the Unix shell.  So if you run to use pipes, or run a sequence of commands as a single `tsp` job, then it is easiest to put those shell commands into a file.  This file is called a shell script.

For example, let's look at the `countlines` shell script:
~~~
$ nano countlines
~~~
{: .bash}

Now let's run this script, along with some sleep commands.  The sleep command does nothing except wait around for the specified number of seconds.

~~~
$ tsp sleep 10
$ tsp bash countlines
$ tsp sleep 2
$ tsp sleep 11
$ tsp
~~~
{. :bash}
~~~
ID   State      Output               E-Level  Times(r/u/s)   Command [run=1/1]
0    running    /tmp/ts-out.IiCgZe                           sleep 10
1    queued     (file)                                       bash countlines
2    queued     (file)                                       sleep 2
3    queued     (file)                                       sleep 11
~~~
{. :output }

## Inspecting results of jobs

Let's assume you have some finished `tsp` jobs:

~~~
$ tsp
~~~
{. :bash}
~~~
ID   State      Output               E-Level  Times(r/u/s)   Command [run=0/1]
0    finished   /tmp/ts-out.XIFxt5   0        10.00/0.00/0.00 sleep 10
1    finished   /tmp/ts-out.MuxTsu   0        0.45/0.30/0.15 bash countlines
2   finished   /tmp/ts-out.9PHhJZ   0        2.00/0.00/0.00 sleep 2
3   finished   /tmp/ts-out.OuJkB4   0        11.00/0.00/0.00 sleep 11
~~~
{. :output }


Note that `tsp` give each job an ID.  You can see the output of a job:

~~~
$ tsp -c 1
~~~
{: .bash}
~~~
41511
~~~
{: .output}

And to see more detailed information about the job:

~~~
$ tsp -i 1
~~~
{: .bash }
~~~
Exit status: died with exit code 0
Command: bash countlines
Slots required: 1
Enqueue time: Thu Jul  6 15:20:57 2017
Start time: Thu Jul  6 15:20:57 2017
End time: Thu Jul  6 15:20:58 2017
Time run: 0.446569s
~~~
{: .output }

### Misc Commands

Remove queued job with ID 2:
~~~
$ tsp -r 2
~~~

Clear list of finished jobs:
~~~
$ tsp -C
~~~

Kill running job:
~~~
$ kill $(tsp -p 2)
~~~
