---
title: "Introduction to the Command Line"
subtitle: "The basics"
date: 2022-10-21T11:05:10-07:00
draft: false
description: Introduction to the commandline. Beginning tutorial
categories: tutorials
tags: [command line, terminal, shell]
---

The command line can feel a bit like a step backwards in terms of user experience, or even intimidating. However, it can be a very powerful tool, and even impossible to avoid if you want to program or be involved in IT. 

## What is it?
Probably the most recognizable image of the command line is that black box on the screen with green text. Basically it is a place to interact with your computer via text rather than photos (mostly). You'll need an application called a "terminal" or "terminal emulator" to get started. Also, you will need to select a shell. Think of it as what interprets the text you type, and the terminal displays it. Each operating system has it's own default shell, but for the most part you have at least a few alternatives. There are plenty of articles written on the subject. So, we are just going to look at some high level concepts. In general a command entered into the terminal is broken into 3 parts.

### executable
This is the name of the commandline application itself.

### flags
Flags are options that you pass to the executable starting with one or two dash(es) to indicate how you would like it to perform. For example, when you pass "--help" to many commandline apps, rather than performing their normal tasks, they display help information on how to use them.

### arguments
Arguments are similar to flags but are nameless and need to go in a particular order. If you wanted to open a webpage in Google Chrome it would look something like:

`google-chrome https://google.com`

with the URL "https://google.com" as an argument

Some applications have flags and argument. If you wanted to open that same page, but in incognito mode:

`google-chrome --incognito https://google.com`

## Terminal
Most OSes will come with some default terminal. 

**Widows** has Powershell which can be accessed via *clicking Start, type PowerShell, and then click Windows PowerShell*. 

**OSX** has Terminal which can be accessed via *clicking the Launchpad icon in the Dock, type Terminal in the search field, then click Terminal*. 

**Linux**, well it varies and if you are using a Linux variant, you probably already know. You can always use Google to lookup your OS's default terminal.

## Command Prompt
The command prompt usually consists of one or two lines of customizable information (username, hostname, cwd, etc), followed by the cursor waiting for user input. The default bash prompt often looks something like this:

```bash
user@host:path$ |

```
How and how much you can configure it will depend on the shell you use. My current preferred shell is [fish shell](https://fishshell.com/). Leave a comment if you would be interested in an article on how to use and configure it.

## Navigation
When using the command line there is a concept of a "current working directory," CWD for short. You can think of this as your current location. you have access to see the files and folders in this location. To reference files outside of this location, you will have to explicitly give the location of said files. For example, if you wanted to look at a file in your `Downloads` folder, but were currently in your `Documents` folder, you would have to specify how to get to the `Downloads` folder from there. In order to learn how to navigate through our directories we need to understand the directory structure.

### Directory Structure
As I am sure you are aware, directories can contain files and directories which in turn can contain more files and more directories. So, where is the starting point? Well that depends on which Operating System you use e.g., Windows, OSX, Linux. But really the main difference will be if you use Windows or not, as Windows is the only one that really does it differently.

#### Windows
Windows has a concept of a base level called `Drives`. They are usually assigned a letter. These drives can represent a part of a disk or solid state drive, CD or DVD drive, or even a remote/network drive. Often the main drive will be assigned as the `C` drive. When trying to reference something in the `C` drive it would start like `c:\`

#### The Other OSes
On OSX (Mac) and linux, there is a concept called the `root` directory. For the most part, every file and directory can be found here. When reference something for the root directory it would start like `/`

### Paths
When giving a location to a file or folder it is ofter referred to as a "path". A path can be "absolute" or "relative"

#### Absolute 
Absolute paths are paths that begin at well, the beginning i.e., the `drive` or `root`. Here are some examples of what a path to `note.txt` in  `Documents` directory may look like:

Windows:
```powershell
c:\Users\jhagofsk\Documents\note.txt
```
Others:
```bash
/home/jhagofsk/Documents/note.txt
```

#### Relative
Relative paths are relative to the current working directory, CWD. To see what the CWD is you can use the `pwd` command (short for print working directory):

![pwd](/img/pwd.gif)

In this case I could reference `note.txt` in my Documents folder as:

Windows:
```powershell
Documents\note.txt
```
Others:
```bash
Documents/note.txt
```

## Listing Contents of Directories
If you want to see what files and folders are in a directory you can either use the `dir` command for Windows Powershell or `ls` command for everything else. Each works very similarly. If you don't pass any arguments it will display the contents of CWD, or if you want you can specify a path to see its contents:

![ls](/img/ls.gif)

## Changing the CWD
Often you will want to change the CWD. You can do this with the `cd` command. Simply type the path you want be the new CWD as an argument. If you wanted the CWD to be your Documents folder:

Windows:
```powershell
cd c:\Users\jhagofsk\Documents
```
Others:
```bash
cd /home/jhagofsk/Documents
```

## Environment Variables
Think of environment variables as configuration values for the command line. I won't go into detail here, but there is one specific variable that we should cover.

### PATH
The `PATH` variable gives the shell a list of directories to search for executables. For instance, if you didn't want to type the full path to the `ls` command every time, you could just make sure the path to it is included in `PATH`. That's why in the previous examples we just typed `ls` rather than something like `/usr/bin/ls`. Modifying environment variables varies from shell to shell, but viewing as specific on is fairly universal. If we wanted to look the the current `PATH`:

![ls](/img/path.gif)

We use the `echo` command followed by the name of the variable with a `$` prepended to it. (Try it without the `$`. What happens?) Here we see a list of paths separated by a colon. So if a path isn't specified, the shell will check all these folders for the file being reference in the command. 

## Conclusion
We just scratched the surface of the command line. There are plenty of shortcuts and tricks that can make your command line experience more efficient and useful. Check back for more articles on just that. Again feel free to leave questions in the comments.

