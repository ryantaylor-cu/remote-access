---
title: "Running a program remotely with a GUI"
teaching: 15
exercises: 0
questions:
- "How can I run a program on a remote computer but still use the graphical user interface that I'm used to?"
- "How can I run a remotely running GUI in the background?"
objectives:
- "First objective."
keypoints:
- "First key point."
---

Mention here that they need to connect with -X. Tell them to ddisconnect and reconnect.

## SPSS

After you have connected to your RCDC virtual machine using ssh, run SPSS with this command:

~~~
statistics
~~~
{: .bash}

## STATA

After you have connected to your RCDC virtual machine using ssh, run stata with this command:

~~~
xstata
~~~
{: .bash}

## Matlab

After you have connected to your RCDC virtual machine using ssh, run graphical Matlab with this command:

~~~
matlab
~~~
{: .bash}

To run Matlab in a text only command-line environment, without a graphical interface and without on-screen plotting:

~~~
matlab -nodisplay
~~~
{: .bash}

# Background GUI Apps with xpra

The xpra program allows you to run X11 programs in the “background”.  This means that you can disconnect and reconnect to them over ssh.

## Installation

To install xpra on Ubuntu Linux, run this command:

~~~
sudo apt install xpra
~~~
{: .bash}

To install xpra on Windows, download and install Xpra-x86_64_Setup.exe from http://xpra.org/trac/wiki/Download.  Finally, from MobaXterm run the following command:

~~~
ln -sv “/drives/c/Program Files/Xpra/Xpra.exe” /bin/xpra
~~~
{: .bash}

## Start Session

The :1234 in my example is an X11 display number, not a network port. This number must be unique to the session. From a Linux terminal or Windows MobaXterm:

~~~
xpra start ssh/134.117.214.123/1234 –start-child=gnome-terminal
~~~
{: .bash}

Xpra will ask you for the username and password that you use on the VM.  Then once the xpra session is established, a gnome-terminal window should appear, from which you can launch further GUI applications.

## Reconnect to Running Session

To attach to the running session:

~~~
xpra attach ssh/134.117.214.123/1234
~~~
{: .bash}

## Stop Session

To stop the running session:

~~~
xpra stop ssh/134.117.214.123/1234
~~~
{: .bash}

## List Sessions

To list running sessions:

~~~
ssh 134.117.214.123 xpra list
~~~
{: .bash}
