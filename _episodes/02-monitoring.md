---
title: "Monitoring resources"
teaching: 15
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
- "Calculate available RAM by including buffered/cached"
- "The `nice` command changes a process' priority."
- "The `top` command shows an interactive display of hardware resources."
- "The `ps` command shows a list of processes"
- ""
---

After you are able to connect to a server and run programs, it is often useful to know how much of the server your programs use.  And servers are often shared, so it is also useful to see what is available on the server. There are three types of server hardware resources to consider - disk space, RAM, and CPU.

## Process

We have seen how you can run commands in the Linux shell.  These commands are software programs, just like your web browser or word processor.  A running instance of a program is represented in Linux as a *process*.

Things get more complicated when one software program makes use of multiple processes.  This allows the program to do more than one task at once.

> ## Threads vs Processes
> To complicate things further, we should also differentiate between a process and a thread. A program may use several processes while running. Each process is a separate path of execution and are, for all intents and purposes, completely independent. A process can be composed of multiple *threads*. These threads are separete paths of execution within a process. Generally speaking, threads are used for smaller tasks and processes are used for heavyweight tasks. This distinction will become important when monitoring processes. 
{: .callout}


## CPU Usage

The system load is a measure of how many processes are running and how computationally intensive those tasks are. The amount of processing power you have will depend on the type of processor in the machine you are using. In all likelihood your computer has a multicoreCPU. These factors are important in determining the current load on a given machine and whether you are able to take advantage of any available resources.

## Memory

Another major aspect to consider when monitoring the load of a given computer is the amount of used and free memory. Every process uses a variable amount of memory and can only function effeciently when there is enough free memory for them to use. Besides the real memory, the computer can also use *virtual memory*, a combination of real memory and disk space. The portion of virtual memory that uses the disk is known as swap space. When the computer has to resort to using the hard drive in place of memory, the performance of the system drops considerably due to the fact that the disk is orders of magnitude slower than memory. It is for this reason that monitoring your memory usage (and ensuring you do not run out) is important. 

## Disk Space

The last resource it is important to monitor is the amount of free disk space. When it comes to scientific computing, it is possible that you have a large ammount of input data that needs to be processed by a series of programs, each of which has a significant amount of output data. On top of this there may be several other people using the machine and disk doing similar tasks. Completely running out of disk space is probably the most catastrophic scenario compared to running out of the other resourses mentioned. This is because the operating system also uses the hard disk frequently to read and write files of all kinds (temporary files, log files, etc). When the disk becomes compeltely full, these basic operating system tasks cannot be done and the whole system will grind to a halt. This can also complicate rebooting the system as well. For these reasons it is important to keep an eye on the amount of disk being used at all times. 
 
<!--
> <span class="glyphicon glyphicon-warning-sign"></span> **FIXME** I
> guess we should expect students to know what RAM is, as well as
> binary units such as MB and GB?
{: .alert .alert-danger}
-->
>  <span class="glyphicon glyphicon-warning-sign"></span> **FIXME** Is
>  it important to mention SWAP vs RAM, OS caching?
{: .alert .alert-danger}

>  <span class="glyphicon glyphicon-warning-sign"></span> **FIXME**
>  Should we discuss a bit about the concept of what a process is?
>  Helps when looking at at top or ps output.  To understand top's CPU
>  use, maybe mention that one process can use multiple cores
>  (multithreading), or users may also see one program split across
>  multiple processes.  Students may also need to be aware that a
>  process might not always use 100% of a core, e.g. if it is waiting
>  for I/O from a disk
{: .alert .alert-danger}

>  <span class="glyphicon glyphicon-warning-sign"></span> **FIXME**
>  Can we rework the commands below to organize them by task (finding a
>  particular process' RAM, then CPU vs finding those system-wide)?
>  Or is it best to go through each command one at a time?
{: .alert .alert-danger}

>  <span class="glyphicon glyphicon-warning-sign"></span> **FIXME**
>  Does the `nice` command fit into this lesson?
{: .alert .alert-danger}


* `nice`: Changes a process' priority
* `top` or `htop`: Both top and htop gives you a dynamic, real-time view of all of the processes running on your system. There is a lot of data displayed for each process. Likely the most important information for you is the %CPU and %MEM details of the processes you are running. If you are running a multithreaded application, %CPU can go over 100%. The max %CPU will be the number of cores assigned to your VM (n) times 100%. If you notice your process (or sum of the CPU usage over you processes) are using close to n00% CPU, you are completely using all available cores. You need to also monitor %MEM. If this gets too high (ie. reaches 100%), your process will crash. If you notice that your process is slowly but continually using more memory it will likely crash eventually.
* `ps <ID>`: Every process running on your system is given a unique ID. The ID assigned to each process is listed in the first column of the top output. This command gives more information on a specific running process.
* `free -h`: This command gives a quick summary of the free and used memory across your VM. Including the option -h displays the output using human readable units.
* `df -h`: This command gives a quick usage summary of the available disk storage. An intervention should be made if you notice any listed volume approaching 100% usage.

