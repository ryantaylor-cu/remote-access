---
title: "Monitoring Resources"
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
- "The `top` command shows an interactive display of hardware resources."
- "The `ps` command shows a list of processes"
- ""
---

After you are able to connect to a server and run programs, it is often useful to know how much of the server your programs use.  And servers are often shared, so it is also useful to see what is available on the server. There are three types of server hardware resources to consider - disk space, RAM, and CPU.

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
>  Can we rework the tasks below to organize them by task (finding a
>  particular process' RAM, then CPU vs finding those system-wide)?
>  Or is it best to go through each command one at a time?
{: .alert .alert-danger}

>  <span class="glyphicon glyphicon-warning-sign"></span> **FIXME**
>  Does the `nice` command fit into this lesson?
{: .alert .alert-danger}



* `top` or `htop`: Both top and htop gives you a dynamic, real-time view of all of the processes running on your system. There is a lot of data displayed for each process. Likely the most important information for you is the %CPU and %MEM details of the processes you are running. If you are running a multithreaded application, %CPU can go over 100%. The max %CPU will be the number of cores assigned to your VM (n) times 100%. If you notice your process (or sum of the CPU usage over you processes) are using close to n00% CPU, you are completely using all available cores. You need to also monitor %MEM. If this gets too high (ie. reaches 100%), your process will crash. If you notice that your process is slowly but continually using more memory it will likely crash eventually.
* `ps <ID>`: Every process running on your system is given a unique ID. The ID assigned to each process is listed in the first column of the top output. This command gives more information on a specific running process.
* `free -h`: This command gives a quick summary of the free and used memory across your VM. Including the option -h displays the output using human readable units.
* `df -h`: This command gives a quick usage summary of the available disk storage. An intervention should be made if you notice any listed volume approaching 100% usage.

