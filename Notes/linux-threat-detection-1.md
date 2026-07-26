# Initial Access via SSH

1) **Common risks when using key-based authentication:**
    - Threat actors access a service or source code where private SSH keys have been stored (Like a GitHub repository or Ansible automation server containing SSH credentials)
    - Threat actors steal SSH keys to a server by infecting an admin's laptop with a data stealer
2) **Additional risks when using password-based authentication:**
    - An IT admin sets a weak SSH password for a quick test and forgets to revert the changes
    - An IT support enables SSH for a contractor who sets the password to "12345678"
    - A network engineer accidentally exposes an old, insecure SSH server to the Internet

# Detecting SSH Attacks

There are three indicators of malicious logins to pay attention to:
1) Multiple failed logins followed by a success
2) Logins from an external untrusted IP address
3) Logins by password, an insecure auth method

To make a final verdict, you might need to investigate more details:

- **Username:** Who owns the user? Is it expected for them to log in at this time and from this IP?
- **Source IP:** What do TI tools and asset lookups say about the IP? Is it trusted or malicious?
- **Login history:** Was the login preceded by brute force or other suspicious system events?
- **Next steps:** Is the login suspicious? Should I analyze user actions following the login?

# Initial Access via Services - Linux and Public Services

Application logs can still provide unique artifacts for analysis. For example, you can:

- Use web logs to detect a variety of web attacks
- Use database logs to detect suspicious SQL queries
- Use VPN logs to detect abnormal VPN login events
- Refer to other logs for specific events like bank transactions

# Auditd and Process Tree

You begin by locating the suspicious command in the logs with `ausearch -i -x COMMAND`. Next, you walk up the process tree using the `--pid` option until you reach PID 1, the OS process.

