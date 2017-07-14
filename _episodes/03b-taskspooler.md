---
title: "Managing jobs with a queue"
teaching: 15
exercises: 0
questions:
- "How can I more easily control many background jobs?"
objectives:
- "Create a queue of waiting jobs"
- "Manage a queue of jobs"
keypoints:
- "The `tsp` program, if available, will schedule a queue of jobs."
---

## Overview

Task Spooler (`tsp`) is not installed on every Linux server.  However, we mention it here because if it happens to be available, it is a straightforward way to run many jobs.

Task Spooler is a type of job queuing software.  There are many more complicated job queueing software packages.  Task Spooler is a particularly simple tool.  It only runs on one server for one user.

The `tsp` command will run a job in the background;  this lets you run other commans while `tsp` is busy running your program.  The `tsp` command will even keep running your jobs after you are logout or disconnect from the server.

In its normal setup, only one job will run with `tsp` at a time.  However, you can ask `tsp` to run more jobs.  Once the first job finishes, `tsp` will automatically start the next job.  This list of waiting jobs is called *queuing*, and is analogous to the lineup at a grocery store.

Let's start taking a more concrete look at how to use the tsp command.

## Listing tsp jobs

You can view your list of tsp jobs:

~~~
$ tsp
~~~
{: .bash}

~~~
ID   State      Output               E-Level  Times(r/u/s)   Command [run=1/1]
1    running    /tmp/ts-out.IiCgZe                           sleep 10
2    queued     (file)                                       sleep 10
0    finished   /tmp/ts-out.QUFZ40   0        2.00/0.00/0.00 sleep 2
~~~
{: .output}

The number from the ID column will be used to refer to particular jobs.  The State column tells you the progress tsp is making in your job queue.


## Running jobs with tsp

Just put `tsp` in front of the command you want to run.  Note that `tsp` can only directly run a single command, but itself does not handle the special functionality of the Unix shell.  So for example if use a command containing pipes or output redirects, then it is easiest to put those shell commands into a file.  This file is called a shell script, but is out of the scope of our current discussion.

Let's again run the `slowoutput` example program, but this time run it using `tsp`:

~~~
$ tsp ./slowoutput 10
$ tsp ./slowoutput 3
$ tsp ./slowoutput 20
$ tsp
~~~
{: .bash}
~~~
ID   State      Output               E-Level  Times(r/u/s)   Command [run=1/1]
0    running    /tmp/ts-out.Smoz2i                           ./slowoutput 10
1    queued     (file)                                       ./slowoutput 3
2    queued     (file)                                       ./slowoutput 20
~~~
{: .output }

We have queued up 3 jobs, each running a slowoutput.  We'll move on now to ways of managing jobs and getting output.


## Inspecting results of jobs

Let's assume you have some finished `tsp` jobs, that looks like this:

~~~
$ tsp
~~~
{: .bash}

~~~
0    finished   /tmp/ts-out.Smoz2i   0        10.04/0.00/0.00 ./slowoutput 10
1    finished   /tmp/ts-out.1UDMiN   0        3.02/0.00/0.00 ./slowoutput 3
2    finished   /tmp/ts-out.twnIHU   0        20.11/0.00/0.00 ./slowoutput 20
~~~
{: .output }


Note that `tsp` give each job an ID.  You can see the output of a job:

~~~
$ tsp -c 1
~~~
{: .bash}
~~~
Example Output 3
Example Error 3
Example Output 2
Example Error 2
Example Output 1
Example Error 1
DONE Output
DONE Error
~~~
{: .output}

And to see more detailed information about the job:

~~~
$ tsp -i 1
~~~
{: .bash }

~~~
Exit status: died with exit code 0
Command: ./slowoutput 3
Slots required: 1
Enqueue time: Fri Jul 14 10:45:28 2017
Start time: Fri Jul 14 10:45:38 2017
End time: Fri Jul 14 10:45:41 2017
Time run: 3.021296s
~~~
{: .output }

### Misc Commands

Remove queued job with ID 2:

~~~
$ tsp -r 2
~~~
{: .bash}

Clear list of finished jobs:
~~~
$ tsp -C
~~~
{: .bash}

Kill running job:
~~~
$ kill $(tsp -p 2)
~~~
{: .bash}
