---
title: "Monitoring Resources"
teaching: 30
exercises: 0
questions:
- "What hardware resources are being used?"
objectives:
- "Understand used and available RAM."
- "Display disk space usage."
- "Examine how many CPU cores are being used."
keypoints:
- "First key point."
---

* `top` or `htop`: Both top and htop gives you a dynamic, real-time view of all of the processes running on your system. There is a lot of data displayed for each process. Likely the most important information for you is the %CPU and %MEM details of the processes you are running. If you are running a multithreaded application, %CPU can go over 100%. The max %CPU will be the number of cores assigned to your VM (n) times 100%. If you notice your process (or sum of the CPU usage over you processes) are using close to n00% CPU, you are completely using all available cores. You need to also monitor %MEM. If this gets too high (ie. reaches 100%), your process will crash. If you notice that your process is slowly but continually using more memory it will likely crash eventually.
* `ps <ID>`: Every process running on your system is given a unique ID. The ID assigned to each process is listed in the first column of the top output. This command gives more information on a specific running process.
* `free -h`: This command gives a quick summary of the free and used memory across your VM. Including the option -h displays the output using human readable units.
* `df -h`: This command gives a quick usage summary of the available disk storage. An intervention should be made if you notice any listed volume approaching 100% usage.

