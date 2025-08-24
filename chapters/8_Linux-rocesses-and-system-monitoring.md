# Linux Processes and System Monitoring

---

[YouTube Video](TBD)

---

This module explores how Linux manages running programs (“processes”) and teaches you to monitor and control your system using official tools and methods. By the end of this section, you’ll be comfortable using both theory and hands-on practice, ready to tackle common problems and optimize system performance.

---

## What is a Linux Process?

A **process** is simply a running instance of a program. Every command you launch, whether visible as an app or running in the background, becomes a process. The system tracks these using unique process IDs (PIDs). Some processes are started or managed by system services, often called **`daemons`**.

Learn more about key topics from the [Ubuntu official documentation](https://documentation.ubuntu.com/server/) and [Red Hat monitoring guide](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/monitoring_and_managing_system_status_and_performance/index).

---

## Foreground and Background Processes

Processes can run in the **`foreground`** or **`background`** of your shell:

- **Foreground processes:** These are tied directly to your terminal. When you start them, you must wait until they finish to get your shell back.
- **Background processes:** These keep running independently of your terminal, freeing you up to run other commands.

To run something in the background, just add `&` at the end:

```bash
sleep 120 &
```

If you accidentally started a command in the foreground, pause it (`Ctrl+Z`), then send it to the background with:

```bash
bg
```

Bring it back to the foreground any time with:

```bash
fg
```

To see a list of background and suspended `jobs` in your shell, use:

```bash
jobs
```

For more, see Red Hat’s blog on the [jobs, bg, and fg commands](https://www.redhat.com/en/blog/jobs-bg-fg).

---

## Viewing and Managing Processes

### GUI: System Monitor

On Ubuntu, you can launch **System Monitor** from your app menu. It provides a graphical view to watch running processes, sort by CPU/Memory, and even stop (`kill`) misbehaving tasks. Red Hat systems also offer a similar tool. View details in the [Red Hat system monitoring guide](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/deployment_guide/ch-system_monitoring_tools).

### Command Line Essentials

- **ps:** Snapshots all or selected processes. Try:
```

ps aux
ps -u yourusername

```
- **top:** Realtime resource monitor to see live CPU, memory usage, and process info. Exit with `q`.
- Look up [the official top manpage](https://manpages.ubuntu.com/manpages/lunar/man1/top.1.html).
- **htop:** More visual, interactive version of `top`. Install with `sudo apt install htop` (Ubuntu) or `sudo dnf install htop` (Red Hat), then just run `htop`.
- **btop:** A modern alternative to `top` with a better interface. Install with `sudo apt install btop` or `sudo dnf install btop`. and run `btop`.

- **pgrep/pidof:** Quickly find process IDs by name. Example:
```

pgrep firefox
pidof bash

```
- **pstree:** Visualizes processes as a tree, showing parent/child relationships.
- **jobs:** Shows background jobs in your shell.
- **fuser:** Identifies which processes are using files or ports.

See more details on each tool in the Red Hat’s [system monitoring documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/monitoring_and_managing_system_status_and_performance/index).

---

## System Resource Monitoring

Linux provides utilities to inspect system-wide resource use:

- **vmstat:** Reports memory, process, and CPU stats. Example:
```

vmstat 2 5

```
- **sar:** Collects and reports system activity; often requires installation.
- **Performance Co-Pilot (pcp):** Advanced analysis and monitoring. Visit RedHat's website to [learn more on pcp ](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/monitoring-performance-with-performance-co-pilot_monitoring-and-managing-system-status-and-performance).
- **free:** Quickly check memory usage.
- **lsof:** See open files by processes.

Find deeper explanations with Red Hat’s [Monitoring System Status & Performance](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/monitoring_and_managing_system_status_and_performance/index).

---

## Identifying Processes for a User

To show all processes for a specific user:
```

ps -u username
top -u username

```
**`htop`** and **`btop`** allows you to sort by user with simple navigation. Learn more about usage at Ubuntu’s and Red Hat’s official system docs on process management.

---

## Limiting the Number of Processes for a User

On multi-user systems (or servers), it’s important to restrict the number of processes to prevent resource exhaustion. This is controlled with settings in `/etc/security/limits.conf`.

For example, to limit user `myuser` to a maximum of 500 processes normally and 1000 at most:
```

myuser soft nproc 500
myuser hard nproc 1000

```
You can view current limits with:
```

ulimit -a

```
Find an official Oracle walkthrough for [Checking Resource Limits for Oracle Software Installation Users](https://docs.oracle.com/en/database/oracle/oracle-database/19/ladbi/checking-resource-limits-for-oracle-software-installation-users.html).

---

## Hands-On Exercises

Try these commands on your Linux machine:

1. **Open “System Monitor”** and find the process consuming the most CPU. Try stopping or killing it (make sure it’s safe to do so!).
2. **List running processes:**  
```

ps aux

```
Notice which processes run under your username.
3. **Practice backgrounding:**
- Start a process in the background: `sleep 200 &`
- Use `jobs` to see it running. Bring it back to foreground with `fg`.
- Use `kill %1` or get PID via `ps` and run `kill PID` to stop it.
4. **Try the advanced tools:**  
Use `vmstat`, `htop`, or `lsof` to monitor or inspect your system.
5. **Set limits (on a test VM):**  
Edit `/etc/security/limits.conf` to set `nproc` for a test user, log in as that user, and try to exceed the limit.

---

## Homework

Answer these questions and try the exercises:

- What’s the difference between a foreground and a background process? How do you move commands between them?
- How would you find and stop a process slowing down your system?
- What tools would you use to troubleshoot high memory or CPU use?
- What is a “zombie” process and how would you identify one?
- Explore at least one system monitoring utility you haven’t used before (see [Red Hat Monitoring & Performance tools](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/monitoring_and_managing_system_status_and_performance/index)).
- Set a user process limit on a test machine and confirm it works as expected by creating many background jobs.

---

You’re now equipped with the fundamental concepts and tools for managing processes and monitoring Linux systems!
