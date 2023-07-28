### Network Commands

* curl 
* wget
* ifconfig 
* ip
* traceroute
* tracepath
* ping
* telnet
* netstat
* ssh
* iperf
* ss
* dig
* nslookup
* route
* host
* arp
* iwconfig
* hostname
* mtr
* whois
* ifplugstatus
* iftop
* tcpdump
* bmon
* nmap
* vnstat

https://itsfoss.com/basic-linux-networking-commands/
https://mindmajix.com/linux-networking-commands-best-examples


### System Processes

https://phoenixnap.com/kb/list-processes-linux
https://www.geeksforgeeks.org/processes-in-linuxunix/
https://www.geeksforgeeks.org/shell-scripting-how-to-send-signal-to-a-processes/

* top
* htop
* ps
* kill
* kilall
* nice

Foreground processes
Background processes ( add & at the end of command)

### Signals


| Signal      |          signal               | explanation   |
| :---        |            :----:              | :---  |
| 1  | SIGHUP                           | If a process is being run from a terminal and that terminal itself is closed/terminated then the process receives this signal and consequently terminates.   |
| 2  | SIGINT                           | It politely tells the program to terminate. Performs the same function as Ctrl+C. It’s up to the process whether it will listen to it or not.   |
| 9  | SIGKILL                         | Unlike other signals, the SIGKILL signal is never sent to the process. Instead, the terminal immediately kills the program and the program doesn’t get the time to save its data or clean up its work. Only use this as a last resort.   |
| 15  | SIGTERM                                     | This is the default signal of the kill command   |
| 18  | SIGCONT                              | This will restore a process that was paused by a SIGSTOP or SIGTSTP signal   |
| 19  | SIGSTOP                               | This signal pauses/freezes the process. The process cannot choose to ignore it.   |
| 20  | SIGTSTP                                   | 	It is similar to SIGSTOP but the process receiving this signal is under no obligation to listen to it. The process may choose to ignore it.   |

	 
		


### What is a service?

A service is a process or application which is running in the background, either doing some predefined task 
or waiting for some event.

/etc/systemd/system/ directory and must have an .service extension. For example, a custom test-app service uses 
/etc/systemd/system/test-app.service unit file.

A unit file is a plain text ini-style file that usually includes three common sections. 
The first section is usually the Unit section which carries generic information about the unit that is not dependent on the type of unit.

The next section is the unit type section, for a service, it is a Service section. 
And the final section is the Install section which carries installation information for the unit.

The following configuration is used to define a service for running a Flask application using Gunicorn, a Python WSGI HTTP Server for UNIX.

vim /etc/systemd/system/test-app.service

```
[Unit]
Description=Gunicorn daemon for serving test-app
After=network.target

[Service]
User=root
Group=root
WorkingDirectory=/apps/test-app/
Environment="PATH=/apps/test-app/bin"
ExecStart=/apps/test-app/bin/gunicorn --workers 9  -t 0  --bind 127.0.0.1:5001 -m 007 wsgi:app --log-level debug --access-logfile /var/log/gunicorn/test_app_access.log --error-logfile /var/log/gunicorn/test_app_error.log
ExecReload=/bin/kill -s HUP $MAINPID
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Description – is used to specify a description for the service.
After – defines a relationship with a second unit, the network.target. In this case, the test-app.service is activated after the network.target unit.
User – is used to specifying the user with whose permissions the service will run.
Group – is used to specify the group with whose permissions the service will run.
WorkingDirectory – is used to set the working directory for executed processes.
Environment – is used to set environment variables for executed processes.
ExecStart – is used to define the commands with their arguments that are executed when this service is started.
ExecReload – is used to define the commands to execute to trigger a configuration reload in the service.
WantedBy – enables a symbolic link to be created in the .wants/ or .requires/ directory of each of the listed unit(s), multi-user.target in this case, when the test-app.service unit is enabled using the systemctl enable command.


systemctl daemon-reload command

systemctl start test-app.service

systemctl status test-app.service

systemctl enable test-app.service

systemctl is-enabled test-app.service

systemctl stop test-app.service

systemctl restart test-app.service

systemd is a Linux initialization system and service manager that includes features like on-demand starting of daemons, 
mount and automount point maintenance, snapshot support, and processes tracking using Linux control groups. 
systemd provides a logging daemon and other tools and utilities to help with common system administration tasks.

systemd is the default init system for the major Linux distributions but is backwards compatible with SysV init scripts. 
SysVinit is an initialization system which predates systemd and uses a simplified approach to service startup. 
systemd not only manages system initialization, but also provides alternatives for other well known utilities, 
like cron and syslog. Because systemd does several things within the Linux user space, 
many have criticized it for violating the Unix philosophy, which emphasizes simplicity and modularity.


To better understand what is meant by an initialization system, this section provides a high-level overview of the Linux boot process.
Linux requires an initialization system during its boot and startup process. At the end of the boot process, the Linux kernel loads systemd and passes control over to it and the startup process begins. During this step, the kernel initializes the first user space process, the systemd init process with process ID 1, and then goes idle unless called again. systemd prepares the user space and brings the Linux host into an operational state by starting all other processes on the system.


The /usr/lib/systemd/user/ directory is the default location where unit files are installed by packages. Unit files in the default directory should not be altered.
The /run/systemd/system/ directory is the runtime location for unit files.
The /etc/systemd/system/ directory stores unit files that extend a service. This directory will take precedence over unit files located anywhere else in the system.


### Linux Boot Process

1. BIOS
BIOS stands for Basic Input/Output System. In simple terms, the BIOS loads and executes the Master Boot Record (MBR) boot loader.
When you first turn on your computer, the BIOS first performs some integrity checks of the HDD or SSD.
Then, the BIOS searches for, loads, and executes the boot loader program, which can be found in the Master Boot Record (MBR). 
The MBR is sometimes on a USB stick or CD-ROM such as with a live installation of Linux.
Once the boot loader program is detected, it's then loaded into memory and the BIOS gives control of the system to it.

2. MBR
MBR stands for Master Boot Record, and is responsible for loading and executing the GRUB boot loader.
The MBR is located in the 1st sector of the bootable disk, which is typically /dev/hda, or /dev/sda, depending on your hardware. 
The MBR also contains information about GRUB, or LILO in very old systems.

3. GRUB
Sometimes called GNU GRUB, which is short for GNU GRand Unified Bootloader, is the typical boot loader for most modern Linux systems.
The GRUB splash screen is often the first thing you see when you boot your computer. It has a simple menu where you can select some options.
If you have multiple kernel images installed, you can use your keyboard to select the one you want your system to boot with. 
By default, the latest kernel image is selected.
The splash screen will wait a few seconds for you to select and option. If you don't, it will load the default kernel image.
In many systems you can find the GRUB configuration file at /boot/grub/grub.conf or /etc/grub.conf. 

4. Kernel
The kernel is often referred to as the core of any operating system, Linux included. It has complete control over everything in your system.
In this stage of the boot process, the kernel that was selected by GRUB first mounts the root file system that's specified in the grub.conf file.
Then it executes the /sbin/init program, which is always the first program to be executed. 
You can confirm this with its process id (PID), which should always be 1.
The kernel then establishes a temporary root file system using Initial RAM Disk (initrd) until the real file system is mounted.

5. Init
At this point, your system executes runlevel programs. At one point it would look for an init file, 
usually found at /etc/inittab to decide the Linux run level.
Modern Linux systems use systemd to choose a run level instead. 


6. Runlevel programs
Depending on which Linux distribution you have installed, you may be able to see different services getting started. 
For example, you might catch starting sendmail …. OK.

These are known as runlevel programs, and are executed from different directories depending on your run level. 
Each of the 6 runlevels described above has its own directory:

Run level 0 – /etc/rc0.d/
Run level 1 – /etc/rc1.d/
Run level 2 – /etc/rc2.d/
Run level 3 – /etc/rc3.d/
Run level 4 – /etc/rc4.d/
Run level 5 – /etc/rc5.d/
Run level 6 – /etc/rc6.d/
Note that the exact location of these directories varies from distribution to distribution.

If you look in the different run level directories, you'll find programs that start with either an "S" or "K" for startup and kill, 
respectively. Startup programs are executed during system startup, and kill programs during shutdown.

Q&A
====
Q - How many steps are in Linux boot process?
There are 6 stages of Linux boot process:
BIOS
MBR
GRUB
Kernel
Init
Runlevel


Q - What is BIOS (basic input/output system)?
The BIOS is the first program executed when a computer starts up. It manages hardware resources such as memory, 
disk drives, video cards, etc. The BIOS also contains code for basic I/O functions like reading from floppy disks, 
printing text on screen, and controlling the keyboard.

Q – How does BIOS work?
The basic input/output system (BIOS) is the first code executed when a computer starts up. 
This initial software loads the operating system into memory and then boots the computer from the hard drive. 
Once the operating system has booted, the BIOS checks for hardware errors such as bad RAM chips, 
faulty power supplies, and other issues. If any problems exist, the BIOS sends error messages to the screen, 
which displays them until they are fixed.

Q – What are the four functions of BIOS?
The functions of BIOS include booting up the computer, loading the operating system, hardware detection, and configuration settings. 
If any one of these functions fails, then the entire computer will stop working.


Q – How to access BIOS?
There are two ways to access the BIOS: 1. Press F2 during boot up 2. Hold down the Del key while powering on the computer.

Q – What is MBR (Master Boot Record)?
The Master Boot Record (MBR) is the first sector of the master boot record on a computer’s hard drive. 
This area contains information about what operating system needs to be loaded next. If any errors occur while loading software from the MBR,
 the computer may crash.

Q – What is Grub?
The grub command line tool allows you to boot from one of the installed operating systems on your computer. 
It also makes it easy to change the default OS used for starting up. This tool has been around since at least 1980s.

Q – What are the difference between Grub and Grub2?
The differences between Grub and Grubs 2 are minimal, but one big change is that Grub2 has been rewritten from scratch. 
This means that all new features were added to Grub2 instead of just adding them to Grub1. Another major difference is that Grub2 uses 
systemd for booting and managing Linux systems.


Q – What is Linux kernel?
The Linux Kernel is the core operating system for all computers using the Linux OS. It provides basic functions such as memory management, 
scheduling tasks, input/output operations, file systems, networking, device drivers, etc.

Q – What is init in Linux?
Init is a process which runs at bootup time and performs various system initialization tasks such as loading modules needed for basic functionality, 
starting daemons and other processes, and performing any one-time initializations.

Q – What is the runlevel in Linux?
The runlevels in Linux are used to control processes during bootup. 

Q – How many runlevels are supported by Linux Operating System?
There are 6 runlevels in Linux Operating System:
run level 0: turn off (shut down) the computer
run level 1: initiate a rescue shell process
run level 2: multi-user mode without networking
run level 3: configure the system as a non-graphical (console) multi-user environment
run level 4: user-definable
run level 5: establish a graphical multi-user interface with network services
run level 6: restart the machine

======================================================================================================================

Job Scheduling on Linux (cron jobs)
------------------------------------

Everyday at 2pm run a command - delete all files older than 12 hours


Job scheduling applications are designed to carry out repetitive tasks as defined in a schedule based on time and event conditions. 
In this article you will learn how to install and start using Cron - the most popular Linux workload automation tool that is widely 
used in Linux community.

What is Cron?
Cron is a Linux job scheduler that is used to setup tasks to run periodically at a fixed date or interval. 
Cron jobs are specific commands or shell scripts that users define in the crontab files. These files are then monitored by the 
Cron daemon and jobs are executed on a pre-set schedule.


Install Cron
Most often Cron is installed to your Ubuntu machine by default. In case it is not there, you may install it yourself.

Update your system’s local package list:

sudo apt update

And install the newest version of cron. The following command also updates Cron to the latest version, if you already have it installed:

sudo apt install cron

Congrats! You now have the latest version of Cron installed on your machine.


Understand How Cron Works
Cron jobs are commands or shell scripts that are referenced in crontab files. These files are loaded into memory and monitored for pre-set 
actions that need to be taken. Cron wakes up every minute to examine all stored crontabs and see if any command needs to be executed in the 
current minute.

Additionally, Cron monitors the modification time of each crontab file on the system. If any crontab has been changed, it is automatically 
reloaded into memory. This way Cron doesn’t need to be restarted when a crontab modification is made.

Manage User-owned Crontab Files
--------------------------------
Users have their own crontab files that are stored in the spool area and named after the user’s account. Each crontab is executed as the user 
who owns the crontab. You can check for user-owned crontab files by listing all files in the spool directory:

ls /var/spool/cron/crontabs

user1
user2



Manage System-wide Crontab Files (root)
---------------------------------
System-wide crontab files are created upon cron installation and mainly used by system services. 
Packages like dpkg, sysstat and many others depend on cron and use it to schedule specific tasks. 
In a system’s crontab there is a user field that is used to execute given tasks.
System crontabs must be owned by root and cannot be edited by any other user. There is no crontab command to edit these files, 
so they can be accessed directly by the system administrator. Let’s now access the main system crontab:

sudo vim /etc/crontab


# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )


https://crontab.guru/

[minute] [hour] [day of month] [month] [day of week] [command]
* * * * * echo “Hello World at $(date)” >> $HOME/greetings.txt




https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/
https://beebom.com/how-enable-systemd-wsl-windows-11/
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-managing_services_with_systemd
https://linuxconcept.com/linux-boot-process-step-by-step-explained/
https://www.freecodecamp.org/news/the-linux-booting-process-6-steps-described-in-detail/
https://www.linode.com/docs/guides/what-is-systemd/
https://www.linode.com/docs/guides/introduction-to-systemctl/

==================================================================================================================


Architecting for resilience and high availability web traffic (Solution Architect)
----------------------------------------------------------------------------------
https://aws.amazon.com/blogs/architecture/lets-architect-creating-resilient-architecture/
https://www.youtube.com/watch?v=KLxwhsJuZ44
https://www.youtube.com/watch?v=62ZQHTruBnk
https://www.youtube.com/watch?v=CDX7oQkuf3A
https://www.youtube.com/watch?v=pbXEH96zhUg

https://aws.amazon.com/resilience-hub/
RTO
RPO



Resilience - robustness
Plenty capacity  - CloudFront (DDOS)

AWS ALB
AWS CloudFront (CDN) - https://aws.amazon.com/cloudfront/
AWS Shield - https://aws.amazon.com/shield/
AWS Shield Advance
AWS Route (DNS)

ASG - self-healing configuration

High availability -  (HA) 

EC2 = ALB + ASG + EC2

===================================================================================================================
===================================================================================================================

Deployment Strategies  (Solution Architect - DevOps Engineer)

https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/deployment-strategies.html
https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/welcome.html?did=wp_card&trk=wp_card

Managing deployments (A/B, canary, Blue/Green), Mutable/Immutable
https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/deployment-strategies.html


Blue/Green - https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf

Canary deployment
The purpose of a canary deployment is to reduce the risk of deploying a new version that impacts the workload. 
The method will incrementally deploy the new version, making it visible to new users in a slow fashion. 
As you gain confidence in the deployment, you will deploy it to replace the current version in its entirety.

Implement Canary Deployment
Use a router or load balancer that allows you to send a small percentage of users to the new version.
Use a dimension on your KPIs to indicate which version is reporting the metrics.
Use the metric to measure the success of the deployment; this indicates whether the deployment should continue or roll back.
Increase the load on the new version until either all users are on the new version or you have fully rolled back.


https://martinfowler.com/bliki/CanaryRelease.html?ref=wellarchitected


A/B, https://vwo.com/ab-testing/
