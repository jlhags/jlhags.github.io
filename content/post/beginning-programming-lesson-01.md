---
title: "Beginning Programming Lesson 01"
subtitle: "Introduction and the Tools"
date: 2022-10-19T09:15:05-07:00
draft: true
description: Beginning programming. The tools to get started.
categories: tutorials
tags: [go, golang, beginning programming, commandline, terminal, ide]
---

When I was about 12 years old, my aunt gave us an old computer. Without going into all the details, I broke it, and all that it would then let you do is write programs in BASIC. It came with a book explaining how to do various things with the language. So having nothing better to do, I worked through it and taught myself how to code, and eventually went to college to learn how to do it better. Now I make a living writing software. I have good news for you. For most people and what they want to do with software development, you don't need to spend tons of money. You can find pretty much everything you need on the Internet. With the tutorials that follow I hope to make it even easier for you. Basic understanding on how to use a computer, browse the Web, and some middle school math is required, but that's pretty much it. We won't go into exhaustive detail on every topic covered in this series, but it should be enough to get you going. So, let's get on with what you will need.
 
## Programming Tools
### The Commandline
While there have been great efforts to build tools that remove the need to access the command line while developing software, chances are you will need to use it at some point, including future lessons.
What is it?
Probably the most recognizable image of the command line is that black box on the screen with green text. Basically it is a place to interact with your computer via text rather than photos (mostly). You'll need an application called a "terminal" or "terminal emulator" to get started. Also, you will need to select a shell. Think of it as what interprets the text you type, and the terminal displays it. Each operating system has it's own default shell, but for the most part you have at least a few alternatives. There are plenty of articles written on the subject. So, we are just going to look at some high level concepts. In general a command entered into the terminal is broken into 3 parts.

#### executable
This is the name of the commandline application itself.

#### flags
Flags are options that you pass to the executable starting with one or two dash(es) to indicate how you would like it to perform. For example, when you pass "--help" to many commandline apps, rather than performing their normal tasks, they display help information on how to use them.

#### arguments
Arguments are similar to flags but are nameless and need to go in a particular order. If you wanted to open a webpage in Google Chrome it would look something like:

`google-chrome https://google.com`

with the URL "https://google.com" as an argument

Some applications have flags and argument. If you wanted to open that same page, but in incognito mode:

`google-chrome --incognito https://google.com`

For more information on the command line check out [Introduction to the Command Line - The Basics](/post/introduction-to-the-commandline)

### Compiler/Interpreter
When you write code you will do so in a specific language that is ideally easy for a human to read. However, it isn't something your computer can understand. So depending on the language you will need a middleman.

#### Compiler
A compiler takes the code you have written and turns it into the language (ones and zeros) your CPU (the processor) understands i.e., creates an executable. This executable will be able to run on any computer that is using a CPU that understand your CPUs language.

#### Interpreter
An interpreter does what it sounds like. It reads the human readable code then explains it to your CPU, so it knows what to do. This code can run on any CPU provided the interpreter is present.


### The IDE
An IDE or Integrated Development Environment is an application that at its core is a text editor similar to Notepad with the addition of other tools that aid in the development of code. For example, most IDEs have a terminal emulator built into them. Also, they will often include code completion, syntax highlighting, and debugging tools. (Those terms will become more meaningful after reviewing future lessons) Many have plugins you can download for additional features, or you can even create your own.

### Version Control Systems
We won't spend a lot on time on this here. However, think of it as something that allows you to keeps track of changes in your code, or really any text based files.

### What to Use
There are a lot of options when it comes to terminals, shells, IDEs, etc. Which to choose can depend what programming language(s) you are using, and preference. In the tutorials that follow, here is what I will be using:

#### [Visual Studio Code](https://code.visualstudio.com/)
This is the IDE I will be using. It comes with terminal built in. So that is one less thing to worry about. Also it has a lot a plugin to choose from to help make your coding experience better.

#### [fish shell](https://fishshell.com/)
This is the shell that I use, but but most of the commands ran will be shell agnostic, so [bash](https://www.gnu.org/software/bash/), [powershell](https://learn.microsoft.com/en-us/powershell/), etc. will work.

#### [Go a.k.a. Golang](https://go.dev/)
Go is the language will will be using so you will need to install it from [here](https://go.dev/doc/install).

#### [git](https://git-scm.com/) 
Git is the VCS that Golang uses, and is very popular choice. Install it from [here](https://git-scm.com/downloads) or better yet see if it is available from your OS's app store/software manager.



### Are You Ready to Begin?
Now that you have an IDE and Golang installed, let's try a couple things to verify it is ready to go.
First open VS Code. Now open the integrated terminal by selecting Terminal -> New Terminal from the top menu. A new panel should open inside the application. Type the following into the terminal, then hit enter:

`go version`

You should see something like the following returned back:

`go version go1.18.2 linux/amd64`

Also check that git is properly installed:

`git version`

You should see something like:

`git version 2.25.1`

![terminal](/img/git_go_version.gif)

If you do, then you are ready to begin! If not, you will need to consult installation documentation. Most likely the executable is not in your defined PATH variable.

## Conclusion
If you would like me to go into more detail on any of the topics listed above or if you have any questions, leave a comment. In the next lesson we will create our first applications and learn about [Variables, Functions, Importing, and Pointers](/post/beginning-programming-lesson-02).
