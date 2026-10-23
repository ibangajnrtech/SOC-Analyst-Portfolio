# Basic Commands to know

- Use `whoami` to find out who you are on linux
- Use `echo` to output some specific text that is provided
- Use `ls` to list what's in the current folder
- Use `cd` to change directory and move into a folder
- Use `cat` to show the contents of a file
- Use `pwd` to print the working director (where am I?)
- Use `find` to search for files by their name
- Use `grep` to search inside a file for text

# Shell Operators (Combining Commands)

- `&` runs commands but does not wait for it to finish before you can do anything else. The command runs in the backgorund, and is helpful for commands that might take a while to complete, or ones that you want to keep running.
- `&&` runs both commands, but waits for the first command to finish first, before the next. Like a set of dominoes.
- `>` is used to redirect output. We can take the output of a command and send it to a file. This operator will overwrite anything that exists in the file.
- `>>` does the same thing, but instead of overwriting, it will just add the output to the bottom of the file.

# Common Directories

**/etc**

This root directory is one of the most important root directories on your system. The etc folder (short for etcetera) is a commonplace location to store system files that are used by your operating system. 

**/log**

This folder stores data that is frequently accessed or written by services or applications running on the system.

**/root**

Unlike the /home directory, the /root folder is actually the home for the "root" system user.

**/tmp**

Short for "temporary", the /tmp directory is volatile and is used to store data that is only needed to be accessed once or twice. Similar to the memory on your computer, once the computer is restarted, the contents of this folder are cleared out.