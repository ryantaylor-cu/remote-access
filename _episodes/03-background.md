---
title: "Jobs in the background"
teaching: 15
exercises: 0
questions:
- "Can I run a job without being connected to the server?"
- "Now that my job is running in the background, how do I control it?"
objectives:
- "Understand background jobs"
- "Manage background jobs"
- "Run jobs when disconnect from a server."
keypoints:
- "Ending a command with `&` puts it in the background."
- "Starting a command with `nohup` keeps the job running while logged out."
- "Redirection with `>` allows you to save program output to files."
- "The `jobs` command lists background jobs."
- "The `kill` command stops a background process."
---

## Shell Jobs

We have now seen that a running program shows up as a process.  You may recall that you can run one shell command that connects programs together using pipes.  A job is the collection of processes that results from the shell command.

The shell is able to do something called job control.  We can actually have more than one background job running at once.

There can be at most one job in the foreground.  This is the job that receives your keyboard input.  When a foreground job finishes, you are returned to the shell prompt, so that you can type more shell commands.

When you are running a background job, it runs without taking control of your shell window and keyboard.  So you can still run other commands at the shell prompt, including many more background jobs, or a foreground job.

## Running Jobs in the Background

Sometimes if a job is going to take a long time to execute, we would like to run this job in the background. This allows us to have our job execute but still lets us have full control over our shell.

To run a job in the background, simply add an `&` to the end of the command line before executing it. For example, we can run out `run_simulation` program for 5 seconds in the background as follows:

~~~
$ ./run_simulation -t 5 &
~~~
{: .bash}
~~~
[1] 24672
~~~
{: .output}
~~~
$ 
~~~
{: .bash}
~~~
[1]+  Done                    ./run_simulation -t 5 &
~~~
{: .output}

When we do this it will launch command and immediately return us to the command prompt.  The process for this job will show up when running the `top` command.

Once the job is done, we will see its output the next time the shell displays its prompt.  In the above example, we just pressed `Enter` after a few seconds, which displayed a new prompt as well as the job done message.

We can also see the shell's list of jobs by using the `jobs` command:

~~~
$ ./run_simulation -t 20 &
$ ./run_simulation -t 18 &
$ jobs
~~~
{: .bash}
~~~
[1]-  Running                 ./run_simulation -t 20 &
[2]+  Running                 ./run_simulation -t 18 &
~~~
{: .output }

If you have already started a job and want to relegate it to the background, press the *Control* and *Z* keys together. This will suspend the job. When you follow this by `bg` it will have it run in the background:

~~~
$ ./run_simulation -t 20
<CTRL-Z>
$ bg
~~~
{: .bash}

Each background job is assigned a job ID. If we want to switch to a background job (ie. bring it to the foreground), we use the command `fg <ID>`. <ID> can be omitted if there is only one background job running.

## Ending a job

If you want to end a job that is running, you can use the `kill <ID>` command. The kill command takes a process ID as a parameter. This ID can be determined using the `ps` or `top` commands.

If the job is your current foreground job, you can end it by pressing the `CTRL-C` key combination.

## Redirecting Job Output to a File

Running certain programs cause a lot of information to be dumped to the console. Sometimes this is done so quickly that we cannot follow what is going on or we would like to save this output for later. To do this, we can use something called a redirect. This redirect causes the information normally dumped to your screen to be saved to a file. You do this by using the `>` symbol.

This will save the output of command to results.txt. This is great, however there are actually two sets of outputs being produced here. The first is the regular output produced by command (known are stdout) as well as all of the errors produced (known as stderr). These “output streams” are normally dumped to the same location so the end user does not know where a given line came from. Sometimes it is useful to separate the stdout from the stderr. We can redirect either of these streams to a file by identifying them with ‘1’ (stdout) or ‘2’ (stderr). To illustrate this we can use our `run_simulation` program again, giving it the -o option. This will tell it to print all of the simulation output to the screen. For example, if we want to save the stdout to a file and have the errors print to screen, we would do the following:

~~~
$ ./run_simulation -o 1> results.txt
~~~
{: .bash}

On the other hand, if we would like to print the stdout to screen and save the errors to a file, we would do the following:

~~~
$ ./run_simulation -o 2> errors.txt
~~~
{: .bash}

If we want to save both streams to separate files, we would do the following:

~~~
$ ./run_simulation -o 1> results.txt 2> errors.txt
~~~
{: .bash}

We can also use the special symbol '&1', which indicates the file that we are sending stdout to.  This symbol can be used to direct both errors and regular output to the same file:

~~~
$ ./run_simulation -o 1> output.txt 2>&1
~~~
{: .bash}

Finally, we can combine this with what we learned in the previous section to save a background job's output:

~~~
$ ./run_simulation 1> results.txt 2> errors.txt &
~~~
{: .bash}

## Keep jobs running after logout

Running jobs in the background a useful way to run many long programs at once.  However, your shell session stops all its jobs when you logout or disconnect.  Even background jobs are stopped.  For jobs that run a long time, they will be stopped if your local computer turns off, or if there are any network interruptions.  It is also convenient to be able to switch between various computers, e.g. at home and school, and continue monitoring your long jobs from both places.

To run a job that can be disconnected from your shell, you should to do two things.  First, tell your job to ignore any disconnect ("hangup" or HUP) signals from your shell using the nohup command.  Second, redirect your output to a file for later viewing.  For example:

~~~
$ nohup ./run_simulation -o -t 300 > results.txt 2> errors.txt &
~~~
{: .bash}

> ## Killing a process
> You have started a process and sent it to run in the background before realizing you need to kill it. How do you go about doing so?
> 
> > ## Solution
> > First you will need to determine the process' ID. To do this you could use `top` or `ps`. With this ID, you can then execute the `kill <ID>` command.
> {: .solution}
{: .challenge}

> ## Running run_simulation and storing the output
> We would like to run `run_simulation` for 10 seconds, store the output in a file called `output.txt`, the errors in a file called `ignore.txt` and have it run in the background. What command should be entered?
>
> > ## Solution
> > ~~~
> > ./run_simulation -t 10 -o 1> output.txt 2> ignore.txt &
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
