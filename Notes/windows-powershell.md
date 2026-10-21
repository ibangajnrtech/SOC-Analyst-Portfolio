# Powershell

PowerShell is a powerful tool from Microsoft designed for task automation and configuration management. It combines a command-line interface and a scripting language built on the .NET framework.

## Objects in Powershell

In PowerShell, objects are fundamental units that encapsulate data and functionality, making it easier to manage and manipulate information. An object in PowerShell can contain file names, usernames or sizes as data (properties), and carry functions (methods) such as copying a file or stopping a process.

## Basic Syntax: Verb-Noun

Powershell commands are known as **cmdlets**. They are much more powerful than the traditional Windows Commands and allow for more advanced data manipulation.
Cmdlets follow a consistent **Verb-Noun** naming convention.

## Basic Cmdlets

- Use `Get-Command` to list all available cmdlets, functions, aliases and scripts that can be executed in the current Powershell session.
- Use `Get-Help` to get detailed information about cmdlets, including usage, parameters and examples (also used to learn how to use a new cmdlet).
- Use `Get-Alias` to list all aliases available.
- Use `Find-Module` to search for modules (collection of cmdlets) in online repositories like Powershell Gallery.

**N/B:** Aliases are shortcuts or alternative names for cmdlets

## Where to Find and Download Cmdlets

- Use `Install-Module` to download and install from the repository.

## Navigating the File System and Working with Files

- Use `Get-ChildItem` to list the files and directories in a location. It can be used to explore directories and view their contents.
- Use `Set-Location` to navigate to a different directory. It changes the current directory, bringing us to the specified path.
- Use `New-Item -Path "\Location\file_name" -ItemType "File"` to create a file in the specified path.
- Use `New-Item -Path "\Location\folder_name" -ItemType "Directory"` to create a folder/directory in the specified path.
- Use `Remove-Item -Path "\Location\file_or_folder_name"` to delete/remove a file or folder from the specified location.
- Use `Copy-Item -Path "\Old_location\file_or_folder_name" -Destination "\New_location\"` to copy files and directories into another location.
- Use `Move-Item -Path "\Old_location\file_or_folder_name" -Destination "\New_location\"` to move files and directories into another location.
- Use `Get-Content -Path "\Location\file_name"` to read and display the contents of a file.

# Piping, Filtering and Sorting Data

Piping is a technique used in command-line environments that allows the output to be used as the input for another.
This creates a sequence of operations where the data flows from one command to the next.

- Use `Where-Object` to filter objects based on specified conditions, returning only those that meet the specified criteria
- Use `Get-ChildItem | Where-Object -Property "Extension" -eq ".file_extension"` to filter the files by their extension property, ensuring that only files with extension equal (-eq) to `.txt` are listed.
- Use `Get-ChildItem | Where-Object -Property "Name" -like "pattern"` to filter files by selecting properties that match the specified pattern.
- Use `Select-Object` to select specific properties from objects or limit the number of objects returned.
- Use `Select-String` to search for text patterns within files.

# Comparison Operators to know

- `-ne` (Not equal)
- `-gt` (Greater than)
- `-ge` (Greater than or equal to)
- `-lt` (Less than)
- `-le` (Less than or equal to)
- `-like` (to match a specified pattern)

# System and Network Information

- Use `Get-ComputerInfo` to retrieve comprehensive computer information, including operating system information, hardware specifications, BIOS and more. `systeminfo` retrieves only a small set of the same details.
- Use `Get-LocalUser` to list all the Local user accounts on the system.
- Use `Get-NetIPConfiguration` to retrieve detailed information about the network interfaces on the system, including IP address
- Use `Get-NetIPAdress` to show details for all IP addresses configured on the system, including those that are not currently active.

# Real-Time System Analysis

- Use `Get-Process` to retrieve detailed view of all currently running processes, including CPU and memory usage
- Use `Get-Service` to retrieve information about the status of services on the machine, such as which services are running, stopped or paused
- Use `Get-NetTCPConnection` to monitor active network connections which displays current TCP connections, giving insights into both local and remote endpoints.
- Use `Get-FileHash -Path file_name_path` to generate file hashes
- Use `Get-Item -Path "file_path" -Stream *` to view the Alternate Data Streams attached to a file

# Scripting

Scripting is the process of writing and executing a series of commands contained in a text file, known as a script, to automate tasks that one would generally perform manually in a shell.

- Use `Invoke-Command` to execute commands on remote systems. Very fundamental for system administrators, security engineers and pentesters.