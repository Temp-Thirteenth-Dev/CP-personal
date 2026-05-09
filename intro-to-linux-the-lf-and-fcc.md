---
description: The Linux Foundation Training course LF101 and Free Code Camp
---

# Intro to Linux The LF and FCC

Locating Applications :\
`which`\
`whereis`

## Accessing Directories

| Command             | Result                                                                                                                                                                                                                 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **pwd**             | Displays the present working directory                                                                                                                                                                                 |
| **cd \~** or **cd** | Change to your home directory; shortcut name is **\~** (tilde)                                                                                                                                                         |
| **cd ..**           | Change to parent directory (..)                                                                                                                                                                                        |
| **cd -**            | Change to previous working directory; **-** (minus)                                                                                                                                                                    |
| pushd               |                                                                                                                                                                                                                        |
| popd                | similar to `cd -`                                                                                                                                                                                                      |
| dirs                | <p>Display the list of currently remembered directories. Directories find their way onto the list with the <code>pushd</code> command; you can get<br>back up through the list with the <code>popd</code> command.</p> |
|                     |                                                                                                                                                                                                                        |

Exploring the Filesystem

| Command                | Usage                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| **cd /**               | Changes your current directory to the root (/) directory (or path you supply)              |
| **ls**                 | List the contents of the present working directory                                         |
| **ls -a**              | List all files, including hidden files and directories (those whose name start with **.**) |
| `ls -l <filename>`     | displays more info about a file.. like access perrmsiion flags, date, time created etc     |
| `ls -l *searchstring*` |                                                                                            |
| `ls -lR`               |                                                                                            |
|                        |                                                                                            |
| **tree**               | Displays a tree view of the filesystem                                                     |
| tree -d                | to view just the directories and to suppress listing file names.                           |

## Hard Links

The **ln** utility is used to create hard links and (with the **-s** option) soft links, also known as symbolic links or symlinks. These two kinds of links are very useful in UNIX-based operating systems.

Suppose that **file1** already exists. A hard link, called **file2**, is created with the command:

**$ ln file1 file2**

Note that two files now appear to exist. However, a closer inspection of the file listing shows that this is not quite true.

**$ ls -li file1 file2**

The **-i** option to **ls** prints out in the first column the inode number, which is a unique quantity for each file object. This field is the same for both of these files; what is really going on here is that it is only one file, but it has more than one name associated with it, as is indicated by the **2** that appears in the **ls** output. Thus, there was already another object linked to **file1** before the command was executed.

![Hard Links](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/rt3q8bc2wdep-lnubuntu.png)

**Hard Links**

Hard links are very useful and they save space, but you have to be careful with their use, sometimes in subtle ways. For one thing, if you remove either **file1** or **file2** in the example, the inode object (and the remaining file name) will remain, which might be undesirable, as it may lead to subtle errors later if you recreate a file of that name.

If you edit one of the files, exactly what happens depends on your editor; most editors, including **vi** and **gedit**, will retain the link _by default_, but it is possible that modifying one of the names may break the link and result in the creation of two objects.

## Soft (Symbolic) Links

Soft (or Symbolic) links are created with the **-s** option, as in:

**$ ln -s file1 file3**\
&#xNAN;**$ ls -li file1 file3**

Notice **file3** no longer appears to be a regular file, and it clearly points to **file1** and has a different inode number.

![Soft (Symbolic) Links](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/nypm7r049h7c-lnsubuntu.png)

**Soft (Symbolic) Links**

Symbolic links take no extra space on the filesystem (unless their names are very long). They are extremely convenient, as they can easily be modified to point to different places. An easy way to create a shortcut from your home directory to long pathnames is to create a symbolic link.

Unlike hard links, soft links can point to objects even on different filesystems, partitions, and/or disks and other media, which may or may not be currently available or even exist. In the case where the link does not point to a currently available or existing object, you obtain a dangling link.

## Viewing Files

**Table: Command Line Utilities Used to View Files**

| Command  | Usage                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **cat**  | <p>Used for viewing files that are not very long; it does not provide any scroll-back.<br><code>-n</code> provides line numbers.</p>                                                                                                                                                                                                                                                                                                                                                                      |
| **tac**  | Used to look at a file backwards, starting with the last line.                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **less** | <p>Used to view larger files because it is a paging program. It pauses at each screen full of text, provides scroll-back capabilities, and lets you search and navigate within the file.<br><br><em><strong>NOTE</strong>: Use <strong>/</strong> to search for a pattern in the forward direction and <strong>?</strong> for a pattern in the backward direction. An older program named more is still used, but has fewer capabilities: "less is more".</em> <code>-n</code> provides line numbers.</p> |
| **tail** | Used to print the last 10 lines of a file by default. You can change the number of lines by doing **-n 15** or just **-15** if you wanted to look at the last 15 lines instead of the default.                                                                                                                                                                                                                                                                                                            |
| **head** | The opposite of **tail**; by default, it prints the first 10 lines of a file.                                                                                                                                                                                                                                                                                                                                                                                                                             |

## touch

**touch** is often used to set or update the access, change, and modify times of files. By default, it resets a file's timestamp to match the current time.

However, you can also create an empty file using **touch**:

```
$ touch <filename>
```

This is normally done to create an empty file as a placeholder for a later purpose.

**touch** provides several useful options. For example, the **-t** option allows you to set the date and timestamp of the file to a specific value, as in:

`$ touch -t 12091600 myfile`

This sets the **myfile** file's timestamp to 4 p.m., December 9th (12 09 1600).

`echo > newfile` can also be used to create an empty new file.

## mkdir and rmdir

**mkdir** is used to create a directory:

* **mkdir sampdir** \
  It creates a sample directory named **sampdir** under the current directory.&#x20;
* **mkdir /usr/sampdir** \
  It creates a sample directory called **sampdir** under **/usr**.

Removing a directory is done with **rmdir.** The directory must be empty or the command will fail. To remove a directory and all of its contents you have to do **rm -rf**.

## Moving, Renaming or Removing a File

Note that **mv** does double duty, in that it can:

* Simply rename a file
* Move a file to another location, while possibly changing its name at the same time.

If you are not certain about removing files that match a pattern you supply, it is always good to run **rm** interactively (**rm –i**) to prompt before every removal.

**Table: Useful Commands**

| Command   | Usage                       |
| --------- | --------------------------- |
| **mv**    | Rename a file               |
| **rm**    | Remove a file               |
| **rm -f** | Forcefully remove a file    |
| **rm -i** | Interactively remove a file |

## Renaming or Removing a Directory

**rmdir** works only on empty directories; otherwise you get an error.&#x20;

While typing **rm –rf** is a fast and easy way to remove a whole filesystem tree recursively, it is extremely dangerous and should be used with the utmost care, especially when used by root (recall that recursive means drilling down through all sub-directories, all the way down a tree).

**Table: Useful Commands**

| Command    | Usage                                     |
| ---------- | ----------------------------------------- |
| **mv**     | Rename a directory                        |
| **rmdir**  | Remove an empty directory                 |
| **rm -rf** | Forcefully remove a directory recursively |

## Modifying the Command Line Prompt

The **PS1** variable is the character string that is displayed as the prompt on the command line. Most distributions set **PS1** to a known default value, which is suitable in most cases. However, users may want custom information to show on the command line. For example, some system administrators require the user and the host system name to show up on the command line as in:

**student@r9 $**

This could prove useful if you are working in multiple roles and want to be always reminded of who you are and what machine you are on. The prompt above could be implemented by setting the **PS1** variable to: **\u@\h $**.

For example:

```bash
$ echo $PS1  
\$  
$ PS1="\u@\h \$ "  
student@r9 $ echo $PS1  
\u@\h \$  
student@r9 $  

```

By convention, most systems are set up so that the root user has a pound sign (**#**) as their prompt.

## Standard File Streams

When commands are executed, by default there are three standard file streams (or descriptors) always open for use: standard input (standard in or **stdin**), standard output (standard out or **stdout**) and standard error (or **stderr**).

**Table: Standard File Streams**

| Name            | Symbolic Name | Value | Example  |
| --------------- | ------------- | ----- | -------- |
| standard input  | **stdin**     | 0     | keyboard |
| standard output | **stdout**    | 1     | terminal |
| standard error  | **stderr**    | 2     | log file |

Usually, **stdin** is your keyboard, and **stdout** and **stderr** are printed on your terminal. **stderr** is often redirected to an error logging file, while **stdin** is supplied by directing input to come from a file or from the output of a previous command through a pipe. **stdout** is also often redirected into a file. Since **stderr** is where error messages (and warning) are written, usually nothing will go there.

In Linux, all open files are represented internally by what are called file descriptors. Simply put, these are represented by numbers starting at zero. **stdin** is file descriptor 0, **stdout** is file descriptor 1, and **stderr** is file descriptor 2. Typically, if other files are opened in addition to these three, which are opened by default, they will start at file descriptor 3 and increase from there.

On the next page and in the chapters ahead, you will see examples which alter where a running command gets its input, where it writes its output, or where it prints diagnostic (error) messages.

## I/O Redirection

Through the command shel&#x6C;**,** we can redirect the three standard file streams so that we can get input from either a file or another command, instead of from our keyboard, and we can write output and errors to files or use them to provide input for subsequent commands.

For example, if we have a program called **do\_something** that reads from **stdin** and writes to **stdout** and **stderr**, we can change its input source by using the less-than sign (**<**) followed by the name of the file to be consumed for input data:

**$ do\_something < input-file**

If you want to send the output to a file, use the greater-than sign (**>**) as in:

**$ do\_something > output-file**

In fact, you can do both at the same time as in:

**$ do\_something < input-file > output-file**

Because **stderr** is not the same as **stdout**, error messages will still be seen on the terminal windows in the above example.

If you want to redirect **stderr** to a separate file, you use **stderr**’s file descriptor number (2), the greater-than sign (**>**), followed by the name of the file you want to receive everything the running command writes to **stderr**:

**$ do\_something 2> error-file**

_**NOTE**: By the same logic,_ _**do\_something 1> output-file** is the same as **do\_something > output-file**._

A special shorthand notation can send anything written to file descriptor **2** (**stderr**) to the same place as file descriptor **1** (**stdout**): **2>&1**.

**$ do\_something > all-output-file 2>&1**

bash permits an easier syntax for the above:

**$ do\_something >& all-output-file**

## Pipes

The UNIX/Linux philosophy is to have many simple and short programs (or commands) cooperate together to produce quite complex results, rather than have one complex program with many possible options and modes of operation. In order to accomplish this, extensive use of pipes is made. You can pipe the output of one command or program into another as its input.

In order to do this, we use the vertical-bar, pipe symbol (**|**), between commands as in:

&#x20;\
&#xNAN;**$ command1 | command2 | command3**

The above represents what we often call a pipeline, and allows Linux to combine the actions of several commands into one. This is extraordinarily efficient because **command2** and **command3** do not have to wait for the previous pipeline commands to complete before they can begin processing at the data in their input streams; on multiple CPU or core systems, the available computing power is much better utilized and things get done quicker.

Furthermore, there is no need to save output in (temporary) files between the stages in the pipeline, which saves disk space and reduces reading and writing from disk, which often constitutes the slowest bottleneck in getting something done.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/1ukn5mq5x9yt-asset-v1_LinuxFoundationXLFS101x1T2023typeassetblockLFS101x_2023_CourseImages-06.png)

**Pipeline**

## Searching for Files

Being able to quickly find the files you are looking for will save you time and enhance productivity. You can search for files in both your home directory space, or in any other directory or location on the system.

The main tools for doing this are the **locate** and **find** utilities. We will also show how to use wildcards in **bash**, in order to specify any file which matches a given generalized request.

### locate

The **locate** utility program performs a search while taking advantage of a previously constructed database of files and directories on your system, matching all entries that contain a specified character string. This can sometimes result in a very long list.

To get a shorter (and possibly more relevant) list, we can use the **grep** program as a filter. **grep** will print only the lines that contain one or more specified strings, as in:&#x20;

**$ locate zip | grep bin**

which will list all the files and directories with both **zip** and **bin** in their name. We will cover **grep** in more detail later. Notice the use of **|** to pipe the two commands together.

**locate** utilizes a database created by a related utility, **updatedb**. Most Linux systems run this automatically once a day. However, you can update it at any time by just running **updatedb** from the command line as the root user.

![locate](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/wvz53j4j1jfx-iproute2.png)

**locate**

### locate vs find

| Feature       | `find`                    | `locate`                                   |
| ------------- | ------------------------- | ------------------------------------------ |
| Speed         | Slow                      | Very fast                                  |
| Accuracy      | Always current            | Depends on database, may not be up-to-date |
| Search method | Scans filesystem          | Uses index/database                        |
| Flexibility   | Very powerful             | Basic name matching                        |
| Best use      | Complex, precise searches | Quick filename lookups                     |

## Wildcards and Matching Filenames

You can search for a filename containing specific characters using wildcards.

**Table: Wildcards**

| Wildcard    | Result                                                                                                                     |
| ----------- | -------------------------------------------------------------------------------------------------------------------------- |
| **?**       | Matches any single character                                                                                               |
| **\***      | Matches any string of characters                                                                                           |
| **\[set]**  | Matches any character in the set of characters, for example **\[adf]** will match any occurrence of **a**, **d**, or **f** |
| **\[!set]** | Matches any character not in the set of characters                                                                         |

To search for files using the **`?`** wildcard, replace each unknown character with **?**. For example, if you know only the first two letters are 'ba' of a three-letter filename with an extension of **.out**, type **ls ba?.out**.

To search for files using the **`*`** wildcard, replace the unknown string with **`*`**. For example, if you remember only that the extension was **.out**, type **`ls *.out`** or `du *.out`

Also used in other places like `sudo apt-get install vmware*`\
Note : If you really want wildcard matching\
Quote the pattern so the shell does not expand it:

```
sudo apt-get install 'vmware*'
```

This lets `apt-get` interpret the pattern rather than your shell.

`sudo apt-cache search wget2`

## The find Program

**find** is an extremely useful and often-used utility program in the daily life of a Linux system administrator. It recurses down the filesystem tree from any particular directory (or set of directories) and locates files that match specified conditions. The default pathname is always the present working directory.

For example, administrators sometimes scan for potentially large core files (which contain diagnostic information after a program fails) that are more than several weeks old in order to remove them.

It is also common to remove files non-essential or outdated files in **/tmp** (and other volatile directories, such as those under **/var/cache/** containing dispensable cached files) that have not been accessed recently. Many Linux distributions use shell scripts that run periodically (through **cron** usually) to perform such house cleaning.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/xiycsz18bcdm-fedorafindlog.png)

**find**

### Using find

When no arguments are given, **find** lists all files in the current directory and all of its subdirectories. Commonly used options to shorten the list include **-name** (only list files with a certain pattern in their name), **-iname** (also ignore the case of file names), and **-type** (which will restrict the results to files of a certain specified type, such as **d** for directory, **l** for symbolic link, or **f** for a regular file, etc.).&#x20;

Searching for files and directories named **gcc**:

**$ find /usr -name gcc**

Searching only for directories named **gcc**:

**$ find /usr -type d -name gcc**

Searching only for regular files named **gcc**:

**$ find /usr -type f -name gcc**

![Using find](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ornov16b9fwp-asset-v1_LinuxFoundationXLFS101x1T2023typeassetblockubuntufindgcc.png)

**Using find**

### Using Advanced find Options

Another good use of **find** is being able to run commands on the files that match your search criteria. The **-exec** option is used for this purpose.

To find and remove all files that end with **.swp**:

**$ find -name "\*.swp" -exec rm {} ';'**

The **{}** (squiggly brackets) is a placeholder that will be filled with all the file names that result from the find expression, and the preceding command will be run on each one individually.

Please note that you have to end the command with either '**;'** (including the single-quotes) or **;**. Both forms are fine.

One can also use the **-ok** option, which behaves the same as **-exec**, except that **find** will prompt you for permission before executing the command. This makes it a good way to test your results before blindly executing any potentially dangerous commands.

![Finding and Removing Files that End with .swp](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/9o95awo7gfra-2024-10-11_12-59-27V2.png)

**Finding and Removing Files that End with .swp**

## Finding Files Based on Time and Size

It is sometimes the case that you wish to find files according to attributes, such as when they were created, last used, etc., or based on their size. It is easy to perform such searches.

To find files based on time:

**$ find / -ctime 3**

Here, **-ctime** is when the inode metadata (i.e. file ownership, permissions, etc.) last changed; it is often, but not necessarily, when the file was first created. You can also search for accessed/last read (**-atime**) or modified/last written (**-mtime**) times. The number is the number of days and can be expressed as either a number (**n**) that means exactly that value, **+n**, which means greater than that number, or **-n**, which means less than that number. There are similar options for times in minutes (as in **-cmin**, **-amin**, and **-mmin**).

To find files based on sizes:

**$ find / -size 0**

Note the size here is in 512-byte blocks, by default; you can also specify bytes (c), kilobytes (k), megabytes (M), gigabytes (G), etc. As with the time numbers above, file sizes can also be exact numbers (**n**), **+n** or **-n**. For details, consult the man page for find.

For example, to find files greater than 10 MB in size and running a command on those files:

**$ find / -size +10M -exec command {} ’;’**

![Finding Files Based on Time and Size](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8vm5v8fixdxu-asset-v1_LinuxFoundationXLFS101x1T2023typeassetblockfindlinuxsize.png)

**Finding Files Based on Time and Size**

## Package Management Systems on Linux

The core parts of a Linux distribution and most of its add-on software are installed via the **Package Management System**. Each package contains the files and other instructions needed to make one software component work well and cooperate with the other components that comprise the entire system. Packages can depend on each other. For example, a package for a web-based application written in Python will require the appropriate Python packages to be installed first.

There are two broad families of package managers widely deployed: those based on Debian and those which use **RPM** as their low-level package manager. The two systems are incompatible but, broadly speaking, provide the same essential features and satisfy the same needs. In addition, there are some other systems used by more specialized Linux distributions.

In this section, you will learn how to install, remove, or search for packages from the command line using these two package management systems.

## Package Managers: Two Levels

Both package management systems operate on two distinct levels: a low-level tool (such as **dpkg** or **rpm**) takes care of the details of unpacking individual packages, running scripts, getting the software installed correctly, while a high-level tool (such as **apt**, **dnf**_,_ or **zypper**) works with groups of packages, downloads packages from the vendor, and figures out dependencies.

Most of the time users need to work only with the high-level tool, which will take care of calling the low-level tool as needed. Dependency resolution is a particularly important feature of the high-level tool, as it handles the details of finding and installing each dependency for you. Be careful, however, as installing a single package could result in many dozens or even hundreds of dependent packages being installed.

![Package Managers: Two Levels](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/syrcqp8yrwo8-asset-v1_LinuxFoundationXLFS101x1T2023typeassetblockLFS101x_2023_CourseImages_6-9-08.png)

**Package Managers: Two Levels**

## Working With Different Package Management Systems

The Advanced Packaging Tool (**apt**) is the underlying package management system that manages software on Debian-based systems. While it forms the backend for graphical package managers, such as the Ubuntu Software Center and synaptic, its native user interface is at the command line, with programs that include **apt** (or **apt-get**) and **apt-cache**.

**dnf** is the open source command-line package-management utility for the RPM-compatible Linux systems that belong to the Red Hat family.

![Working with Different Package Management Systems](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/kmxu4aiinamg-LFS101x_2023_CourseImages_6-9-09.png)

**Working with Different Package Management Systems**

**zypper** is the package management system for the SUSE/openSUSE family and is also based on RPM. **zypper** also allows you to manage repositories from the command line. **zypper** is fairly straightforward to use and closely resembles **dnf**.

**Table: Basic Packaging Commands**

| Operation                         | rpm                                   | deb                         |
| --------------------------------- | ------------------------------------- | --------------------------- |
| Install package                   | **rpm -i foo.rpm**                    | **dpkg --install foo.deb**  |
| Install package, dependencies     | **dnf install foo**                   | **apt install foo**         |
| Remove package                    | **rpm -e foo.rpm**                    | **dpkg --remove foo.deb**   |
| Remove package, dependencies      | **dnf remove foo**                    | **apt autoremove foo**      |
| Update package                    | **rpm -U foo.rpm**                    | **dpkg --install foo.deb**  |
| Update package, dependencies      | **dnf update foo**                    | **apt install foo**         |
| Update entire system              | **dnf update**                        | **apt dist-upgrade**        |
| Show all installed packages       | **rpm -qa** or **dnf list installed** | **dpkg --list**             |
| Get information on package        | **rpm -qil foo**                      | **dpkg --listfiles foo**    |
| Show packages named **foo**       | **dnf list "foo"**                    | **apt-cache search foo**    |
| Show all available packages       | **dnf list**                          | **apt-cache dumpavail foo** |
| What package is **file** part of? | **rpm -qf file**                      | **dpkg --search file**      |

## Documentation sources

## Processes

`ps`

`ps lf`

`ps -f`

`ps -l`

`ps -elf`

`ps aux`

`pstree`

`top`

`at` : schedule future tasks

`cron`

`sleep`

changing nice value priority ?\
`renice +5 7055`\
`renice <incr/decr value> <pid>`

Higher nice value.. lower the priority.\
some need `sudo` permission to increse priority

### Load avgs

### Background and jobs

### top command utility

***

## END Card

***
