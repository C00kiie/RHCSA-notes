<h1> Chapter 8: Linux Processes and Tasks Scheduling </h1>

Objectives:

- Identify and display system and user executed processes
- View process states and priorities
- Examine and change process niceness and priority
- Understand signals and their use in controlling processes
- Review job scheduling
- Control who can schedule jobs
- Schedule and manage jobs using at
- Comprehend crontab and understand the syntax of crontables
- Schedule and manage jobs using cron
- Understand anacron and its function

<h2> Section A: Processes </h2>
Processes are a provisioning worker that utilizes the system resources. There are a few stages for the processes:

- running
- sleeping: sleeping process 
- waiting: io-blockage in general (reading from desk, waiting input .. etc) 
- stopped
- zombie: The process is dead. A zombie process exists in the process
table alongside other process entries, but it takes up no resources. Its entry
is retained until its parent process permits it to die. A zombie process is
also called a defunct process.

<h3> Using Ps </h3>
Couldn't get eaiser with `ps`
- ps returns a tree-like structure with --forest
- e means everything
- a means everything except the user invoking the command 
- u effective user id
- g group
- w unlimited output width

You can also sort the tables with the `-o` pretty arg, like: `ps -o comm,pid,ppid,user` which is cool and better than using awk with ps

<h3> Listing things </h3>

- pidof, pgrep are just ps with a grep that only picks the pid. Don't use it, it's stupid. Be a gentleman and use `ps auxw | grep $ProcessName ` 
- ps -U root lists processes owned by root, likewise for any other user.


<h2> Society of Processes, and being nice </h2>
In general in life, the less of a nice person you are the more likely you are to exploit others and use whatever they have, right? The same goes for processes nice-ness.
It goes from being an absolute douche (-20) and being on the top of priority, and then being a yes-man +20 (the same guy that accepts being friendzoned, too)

how to use it? It's simple

- `nice` displays current default "mask" value for niceness
- `nice -n $VAL $Command` sets a certain command priority pre-startup
- `sudo renice $VAL $PID` resets the value of nice for said $PID

<h3> Kill Signals </h3> 

There are lots of kill signals (such psychotic thing for processes :<) but only a few are useful. You can see all the kill signals that there are in existence by typing `kill -l`

- 1 / sighup hangs up a process, also instructs a daemon to re-read its daemon configuration without restarting (or crashing)
- 2 / sigint CTRL+C signal
- 9 / sigkill literally means DIE
- 15 / sigterm nice way to say sigkill.
- 18 / sigscont same as using bg 
- 20 / SIGTSTP same as using fg






<h1> Job Scheduling using CRON and AT </h1>

<h2> cron </h2>

Cron is responsible for running reptitive tasks at specific times. At boot time the kernel reads files @ `/var/spool/cron` & /etc/cron.d and evaluates the tasks and execute them at the right time.

- cron is enabled for all users, except that it can be managed from at.(allow/deny) and cron.(allow/deny) files in /etc/cron.d. Initially these files don't exist
  but if they exist, they manage who does and doesn't run cron/at (crond/atd services) 
- cron log can be found at `/var/log/cron` which exists only for cron logs.


<h2> At </h2>

- `at -f /opt/script.sh 12:00am` runs a script @ today @ 12:00AM
- `at 11:30pm 3/21/2021` folllowed up by a command prompt lets you type commands to be ran 

It's pretty simple but it's dope and useful

to list at jobs simply type `at -l`
