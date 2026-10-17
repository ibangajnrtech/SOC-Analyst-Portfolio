# System Configuration and Advanced System Settings

The **System Configuration** utility (MSConfig) is for advanced troubleshooting, and its main purpose is to help diagnose startup issues. 
The utility has five tabs across the top namely:
- General
- Boot
- Services
- Startup
- Tools

1)  In the General tab, we can select what devices and services for Windows to load upon boot. The options are: Normal, Diagnostic, or Selective. 

2) The Services tab lists all services configured for the system regardless of their state (running or stopped). A service is a special type of application that runs in the background.

3) In the Startup tab, Microsoft advises using Task Manager (taskmgr) to manage (enable/disable) startup items. The System Configuration utility is NOT a startup management program.

4) In the Tools tab, there is a list of various utilities (tools) that we can run to configure the operating system further. There is a brief description of each tool to provide some insight into what the tool is for. To run a tool, we can use the command to launch the tool via the run prompt, command prompt, or by clicking the `Launch` button. 

## Advanced System Settings

Windows gives you some additional configuration settings as well, which you can use to control the performance behavior and system recovery. To access this option, you can search for `View advanced system settings` in your search bar and open it. This will open a `System Properties` panel.
Windows uses a page file as an extra virtual memory space when the physical RAM becomes full. This helps to prevent slowdowns or application crashes when the system runs out of memory. You can view or modify the page file by navigating to the `Advanced` option at the top and clicking `Settings` under the `Performance` tab.

# Computer Management

The **Computer Management** (compmgmt) utility has three primary sections: **System Tools**, **Storage**, and **Services and Applications**.

# System Information

**System Information** (msinfo32) is a tool that gathers information about your computer and displays a comprehensive view of your hardware, system components and software environment which can be used to diagnose computer issues.

# Resource Monitor

**Resource Monitor** (resmon) displays per-process and aggregate CPU, memory, disk and network usage information, in addition to providing details about which processes are using individual file handles and modules.

# Registry Editor

The **Windows Registry** (regedit) is a central hierarchical database used to store information necessary to configure the system for one or more users, applications, and hardware devices.
The registry contains information that Windows continually references during operation, such as:
- Profiles for each user
- Applications installed on the computer and the types of documents that each can create
- Property sheet settings for folders and application icons
- What hardware exists on the system
- The ports that are being used.