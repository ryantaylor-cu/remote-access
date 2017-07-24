---
title: "Additional Topics"
teaching: 5
exercises: 0
questions:
- "What else could be useful?"
objectives:
- "Learn about network storage."
- "Learn about background graphical applications."
keypoints:
- "Consider using network storage, such as Carleton's research storage drive."
- "The `xpra` command can be used for background GUI applications."
---
## Using Network Storage

Your Linux server will have its own hard drive where your files and programs are saved.  There may time times when you want to use some kind of network storage, because:

-   You can connect to the network storage from multiple computers in different locations
-   Your network storage may be bigger than your linux server's drives.
-   In our case, our network research storage is backed up

Further instructions on using Carleton's research storage:

- [From a Mac](https://carleton.ca/rcs/documentation/connecting-to-rcdc-storage-server-mac/)
- [From a Windows PC](https://carleton.ca/rcs/documentation/connecting-to-rcdc-storage-server-win10/)
- [From a RCDC Linux VM](https://carleton.ca/rcs/documentation/connecting-to-rcdc-storage-server-vm/)


## Background Graphical applications

Sometimes you may want to run a GUI application for a long time, especially if you are running some code or doing a big computation.  There are often ways to do this from the Linux shell.  But if you are running GUI applications as we previously showed you, there is a way to put the GUI application in the background.  This lets you disconnect from the server, and reconnect later, all while your application keeps running.  This requires a program called `xpra`.

Further instructions on xpra can be found on [our website](https://carleton.ca/rcs/documentation/background-gui-apps-with-xpra/).

