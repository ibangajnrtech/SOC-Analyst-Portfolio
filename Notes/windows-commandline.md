# Basic System Information

We can only issue the commands within the Windows Path. You can issue the command `set` to check your path from the command line.

- Use `ver` command to determine the operating system (OS) version.
- Run `systeminfo` command to list various information about the system such as OS information, system details, processor and memory.
- Pipe the command output through `more` if the output is too long like this: `driverquery | more`, use space bar button to view it page by page and exit using `CTRL + C`.
- `help` command provides help information for a specific command
- `cls` command clears the command prompt screen

# Network Troubleshooting

The command-line interface provides many networking-related commands to look up your current configuration, check ongoing connections, and troubleshoot networking issues.

## Network Configuration

- `ipconfig` command can be used to check your network information.
- `ipconfig /all` command can be used to get more information about your network configuration.

## Network Troubleshooting

- `ping target_name` command can be used to send a specific ICMP packet and listen for a response. This is used for checking if a server can be access on the internet. If a reponse is received, we know that we can reach the target and the target can reach us.
- `tracert target_name` command traces the network route traversed to reach the target.

## More Networking Commands

- `nslookup target_name` command looks up a host or domain and returns its IP address.
- `netstat` command displays current network connections and listening ports. `netstat` command with no arguments will show you established connections.
- Run `netstat -h` to display the help page of `netstat`.
- `netstat -a` displays all establlex connections and listening ports
- `netstat -b` shows the program associsted with each listening port and established connection
- `netstat -o` reveals the process ID (PID) associated with the connection
- `netstat -n` uses a numerical form for addresses and port numbers

# File and Disk Management

- `cd` command without parameters displays the current drive and directory
- Use `dir` to view the child directories and files
- Use `dir /a` to display hidden and system files as well
- Use `dir /s` to display files in the current directory and all subdirectories
- Use `tree` to visually represent the child directories and subdirectories
- Use `cd target_directory` to change to any directory
- Use `cd ..` to go up one level
- Use `mkdir directory_name` to create a directory
- Use `rmdir directory_name` to delete a directory

## Working with Files

- Use `type` command to view text files
- Pipe the output of `type` command through `more` to display a single page and wait for you to press spacebar to more by one page
- `copy` command allows you to copy files from location to another
- `move` command can be used to move files to a different location.
- Use `del` or `erase` to delete a file

**TIP:** Use the wildcard character `*` to refer to multiple files.

# Task and Process Management

- Use `tasklist` to list the running processes
- Use `tasklist /?` to check all available filters by displaying the help page
- Use `taskkill /PID target_pid` to terminate any task

# Other important commands to know as a beginner

- `chkdsk` checks the file system and disk volumes for errors and bad sectors
- `driverquery` displays a list of installed device drivers
- `sfc /scannow` scans system files for corruption and repairs them if possible