# Working with Logs in Linux

Unlike in Windows, Linux logs most events into plain text files. This means you can read the logs via any text editor without the need for specialized tools like Event Viewer. On the other hand, default Linux logs are less structured as there are no event codes and strict log formatting rules. Most Linux logs are located in the /var/log folder.

# Authentication Logs

The first and often the most useful log file you want to monitor is /var/log/auth.log (or /var/log/secure on RHEL-based systems). Although its name suggests it contains authentication events, it can also store user management events, launched sudo commands, and much more!

## Login and Logout Events

There are many ways users authenticate into a Linux machine: locally, via SSH, using "sudo" or "su" commands, or automatically to run a cron job. Each successful logon and logoff is logged, and you can see them by filtering the events containing the "session opened" or "session closed" keywords.
In addition to the system logs, the SSH daemon stores its own log of successful and failed SSH logins. These logs are sent to the same auth.log file, but have a slightly different format. You can see them by filtering the events containing the "Failed password" or "Accepted publickey" keywords.

## Other User Management events

You can also use the same log file to detect user management events. This is easy if you know basic Linux commands: If useradd is a command to add new users, just look for a "useradd" keyword to see user creation events!
Other commands include:
- `passwd`
- `userdel`
- `usermod`
- `useradd`

# Common Linux Logs

Linux keeps track of many other events scattered across files in `/var/log`: kernel logs, network changes, service or cron runs, package installation, and many more. Their content and format can differ depending on the OS, and the most common log files are:
- `/var/log/kern.log`: Kernel messages and errors, useful for more advanced investigations
- `/var/log/syslog (or /var/log/messages)`: A consolidated stream of various Linux events
- `/var/log/dpkg.log (or /var/log/apt)`: Package manager logs on Debian-based systems
- `/var/log/dnf.log (or /var/log/yum.log)`: Package manager logs on RHEL-based systems

# Using Auditd

Auditd (Audit Daemon) is a built-in auditing solution often used by the SOC team for runtime monitoring.