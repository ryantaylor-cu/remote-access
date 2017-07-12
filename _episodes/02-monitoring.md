---
title: "Monitoring resources"
teaching: 30
exercises: 0
questions:
- "What hardware resources are being used?"
objectives:
- "Understand the computing concept of a process"
- "Differentiate types of RAM use"
- "Understand used and available RAM."
- "Display disk space usage."
- "Examine how many CPU cores are being used."
keypoints:
- "The `top` command shows an interactive display of hardware resources."
- "The `free` command shows memory use."
- "The `df` command shows available disk space."
- "The `nice` command changes a process' priority."
- "The `ps` command shows a list of processes"
- ""
---

After you are able to connect to a server and run programs, it is often useful to know how much of the server your programs use.  And servers are often shared, so it is also useful to see what is available on the server. There are three types of server hardware resources to consider - disk space, RAM, and CPU.

Let's start by looking at the `top` command.  This is an interactive command that show what is currently running on your server, as well as how much memory and CPU is being used.

From your server shell prompt, start the top program:

~~~
$ top
~~~
{: .bash}
~~~
top - 14:14:43 up 34 days, 22:24,  2 users,  load average: 0.00, 0.06, 0.05
Tasks: 133 total,   1 running, 132 sleeping,   0 stopped,   0 zombie
%Cpu(s): 49.9 us,  0.0 sy,  0.0 ni, 50.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.1 st
KiB Mem :  7980368 total,   216248 free,  1120544 used,  6643576 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  6487508 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
23097 ryantay+  20   0 1071500 1.001g   1444 S 200.0 13.2   0:14.43 useres
23100 ryantay+  20   0   40524   3660   3044 R   0.3  0.0   0:00.01 top
    1 root      20   0   37764   5736   3908 S   0.0  0.1   0:25.26 systemd
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.02 kthreadd
    3 root      20   0       0      0      0 S   0.0  0.0   0:00.16 ksoftirqd/0
    5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
~~~
{: .output}

At any time you may quit top by typing `q`.

## Process

We have seen how you can run commands in the Linux shell.  These commands are software programs, just like your web browser or word processor.  A running instance of a program is represented in Linux as a *process*.

In the `top` command's output, we can see a list of processes in the bottom section of the display.

Each process has a unique pid (process identifier).  We will occasionally need to use these numbers in other shell commands.

## CPU Usage

The system load is a measure of how many processes are running and how computationally intensive those tasks are. The amount of processing power you have will depend on the type of processor in the machine you are using. In all likelihood your computer has a multicore CPU. These factors are important in determining the current load on a given machine and whether you are able to take advantage of any available resources.

In the `top` output, you will see a line in the top summary for '#Cpu(s)'.  This indicates the average CPU use over the last few seconds.  It is the percentage used of all CPUs combined, so it will range from 0% to 100%.  This will show you how many CPU cores would be available if you are about to start a program.

Let's run the `userres` example program to see how we can monitor an individual process' CPU use.

~~~
$ ./useres -c 2
~~~
{: .bash}

In top's listing of processes, you should see a `useres` process that is using 200% CPU.  For processes, the %CPU is the percentage of a single core being used.  So 200% indicates that rougly 2 cores are being used, on average.

## Memory

Another major aspect to consider when monitoring the load of a given computer is the amount of used and free memory. Every process uses a variable amount of memory and can only function efficiently when there is enough free memory for them to use. Besides the real memory, the computer can also use *virtual memory*, a combination of real memory and disk space. The portion of virtual memory that uses the disk is known as swap space. When the computer has to resort to using the hard drive in place of memory, the performance of the system drops considerably due to the fact that the disk is orders of magnitude slower than memory. It is for this reason that monitoring your memory usage (and ensuring you do not run out) is important.

To see how much memory is available, we can return to the `top` program's display.  The 'KiB memory' and 'KiB swap' lines describe memory use.  You can see how much memory you have in the 'total' number.  The 'avail mem' number shows the number of kilobytes of memory that would is available to processes.  This is not the same as the 'free' number, which does not take into account the memory used by Linux itself.

The same information about memory can be seen by running this shell command to use 500MB of memory:

~~~
free -h
~~~
{: .bash}

This `free` command gives a quick summary of the free and used memory across your VM. Including the option -h displays the output using human readable units.

Back in the `top` display, we see a '%MEM' column for each process.  If we run this example command:

~~~
./useres -m 1000
~~~
{: .bash}

It might be easier to see the useres if we sort by memory used.  Do this by pressing `M`.  You can go back to the default CPU sort by pressing `P`.

## Disk Space

The last resource it is important to monitor is the amount of free disk space. When it comes to scientific computing, it is possible that you have a large ammount of input data that needs to be processed by a series of programs, each of which has a significant amount of output data. On top of this there may be several other people using the machine and disk doing similar tasks. Completely running out of disk space is probably the most catastrophic scenario compared to running out of the other resourses mentioned. This is because the operating system also uses the hard disk frequently to read and write files of all kinds (temporary files, log files, etc). When the disk becomes compeltely full, these basic operating system tasks cannot be done and the whole system will grind to a halt. This can also complicate rebooting the system as well. For these reasons it is important to keep an eye on the amount of disk being used at all times. 

The disk free command gives a quick usage summary of the available disk storage. An intervention should be made if you notice any listed volume approaching 100% usage.

~~~
df -h ~
~~~
{: .bash}
~~~
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        78G   25G   53G  32% /
~~~
{: .output}

## Playing nicely with other users

You may want to run a program that takes long time and uses most of the CPU cores in your server.  If other users may need to run smaller programs at the same time, it is polite to lower your program's priority.  This will cause your program to slow down a bit more than the other user's.

~~~
$ nice ./useres -c 4
~~~
{: .bash}

The `nice` command will change a process' priority by increasing its `nice` value.  You can see process' nice values in the 'NI' column of top.

## Process Status command

The `ps` command will show you a list of your processes, similar to the process list in the `top` command's display:

~~~
$ ps u
~~~
{: .bash}
~~~
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
jane     22825  0.2  0.0  21396  5248 pts/0    Ss   14:06   0:00 -bash
jane     22875  0.2  0.0  21468  5436 pts/1    Ss   14:06   0:00 -bash
jane     22920  176 13.1 1071500 1050084 pts/1 Sl+  14:07   0:14 ./useres -c 2 -m 1024
jane     22926  0.0  0.0  36088  3316 pts/0    R+   14:07   0:00 ps u
~~~
{: .output}
