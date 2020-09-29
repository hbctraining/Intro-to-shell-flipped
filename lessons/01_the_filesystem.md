---
title: "The Shell"
author: "Sheldon  McKay, Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu"
date: "September 28, 2020"
---

## Learning Objectives
- How do you access the shell?
- How do you use it?
  - Getting around the Unix file system
  - looking at files
  - manipulating files
  - automating tasks
- What is it good for?

## Setting up

We will spend most of our time learning about the basics of the shell command-line interface (CLI) by exploring experimental data.

Since we are going to be working with this data on the **O2** cluster, we first need to log in.

Before we do either of those things, let's take a quick look at the basic architecture of a cluster environment.

<img src="../img/compute_cluster.png" width="600">

In the above image are represented all the computers that make up a **"cluster"** of computers. Each computer is usually a lot more powerful than any laptop or desktop computer we are used to working with and is referred to as a **"node"** (instead of computer). Each node has a desginated role, either for logging in or for performing computational analysis/work. A given cluster will usually have a few login nodes and several compute nodes.

The data on a cluster is also stored differently than what we are used to with our laptops and desktops, in that it is not computer- or node-specific storage, but all of the data is available to all the nodes in a cluster. This ensures that you don't have to worry about which node is working on your analysis.

We will be going into a lot more depth about the cluster architecture, storage systems and best practices on the last day of this workshop. For now, it a basic understanding is sufficient to get started with learning to interact with a computer using shell commands. Let's learn how to connect to O2.

### Logging in to O2

#### O2 accounts

All clusters require you to have an account to be able to log in. In that way it is similar to any apps that store your data or information. 

For this workshop we will be using training accounts created for us by the HMS Research Computing team, they are the folks that manage the O2 cluster. Each of us should have our own training account associated with a password. These training accounts will be available for workshop-related activities from today until the last day of the workshop.

> If you are interested in getting your own account on O2, please follow the intructions [provided here](https://wiki.rc.hms.harvard.edu/display/O2/Frequently+Asked+Questions+and+Answers#FrequentlyAskedQuestionsandAnswers-Accountsandloggingin) after this workshop.


#### Tool(s) to access remote computers/clusters

**With Mac OS**

Macs have a utility application called "**Terminal**" for performing tasks on the command line (shell), both locally and on remote machines. We will be using it to log into O2. 

Please find and open the Terminal utility on your computers using the *Spotlight Search* at the top right hand corner of your screen.

**With Windows OS**

By default, there is no built-in terminal for the bash shell available with the Windows OS. So, we will be using a downloaded program called "**Git BASH**". Git BASH which is part of the [Git for Windows](https://git-for-windows.github.io/) download is a so-called shell (bash) emulator. What this means is that it shows you a very similar interface to, and provides you the functionality of the Terminal on Mac/Linux OS.

Please find and open Git BASH.

> Windows users can use another program called [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) instead of a bash emulator to log in to remote machines, but it is a little more involved and has different capabilities. We encourage you to take a look at it, but we will not be covering it in this workshop.

#### Let's log in! 

Everyone should have their Terminal (or Git BASH Terminal) window open. Using this Terminal window, you can interact with your own computer using bash commands! You see the "$" symbol? That is where you write the "commands" that will be executed by shell (bash in this case) and your computer's kernel. The "$" is called the **"command prompt"**. 

To connect to the login node on O2, we need to type in the `ssh` command at the command prompt followed by a space, and then type your username plus the address of the cluster, i.e `@o2.hms.harvard.edu`. There is no space between the username and the "@" symbol (see below).

```bash
ssh username@o2.hms.harvard.edu
```

> **Tip** - Syntax for commands on the command-line interface is `command` <space> `arguments`.

Now press the return key (enter), you should receive a prompt for your password. Type in your password and note that **the cursor will not move as you type** it in! This is normal and know that the computer is receiving and transmitting your typed password to the remote system, i.e. the O2 cluster.

If this is the first time you are connecting to the cluster, **a warning will pop up** and will ask you if you are sure you want to do this; **type `Yes` or `Y`**. 

Once logged in, you should see the O2 icon, some news, and a new command prompt: 

```bash
[rc_training10@login01 ~]$ 
```

The command prompt on O2 will have some characters before the `$`, something like `[rc_training01@login01 ~]`, this is telling you the name of the login node you have connected to!

Please note that from this point on in the workshop anything we want you to type next to the command prompt will be preceded by the `$` (see below). Please make sure you do not type out (or copy and paste) the `$` as part of your command

#### Let's move from the login node to a compute node!

The first command we will copy and paste in front of the command prompt will be to start a so-called "interactive session" on O2. This command will connect us to a compute node, so that all of the commands we run will be processed by a computer designated to do analysis.

```bash
$ srun --pty -p interactive -t 0-2:00 --mem 1G --reservation=HBC /bin/bash
```
 
Press enter after you copy and paste in that command. You should see a couple of messages, and in a few seconds you should get back the command prompt `$`, but the string of characters before the command prompt should have changed. They should say something like `[rc_training01@compute-a-16-73 ~]`. This is telling you that you are using one of the compute nodes/computer on the cluster now.

> **Tip** - When you run any command in shell, once the command has finished doing what it is supposed to do, it will bring you back your command prompt.

Once you make sure that your command prompt is preceded by a character string that contains the word "compute" we will copy over some data from a shared location on the cluster to a folder designated to each one of us. By default, when you log in you will automatically be looking at the main folder designated for your use, it is referred to as you "home directory".

> NOTE: When you run the `srun` command between the classes and after this workshop with your own account please remove the `--reservation` string.
> 
> `srun --pty -p interactive -t 0-2:00 --mem 1G /bin/bash`
> 
> The "reservation" is only active for the training accounts during class.

### Copying example data folder

Now that we are all set up to use O2, the first thing to do is to check if there are any files in the data folder we are currently in. To do this we will run a command called `ls` or list.

```bash
$ ls
```

It should show you that you have 0 files, or not show you anything at all because you don't have any data there as yet!

Let's bring in a data folder from a different location on the cluster to our designated area by using the `cp` command. Copy and paste the following command all the way from `cp` and including the period symbol at the end `.`:

```bash
$ cp -r /n/groups/hbctraining/unix_lesson/ .
```

>'cp' is the command for copy. This command required you to specify the location of the item you want to copy (/groups/hbctraining/unix_lesson/) and the location of the destination (.); please note the space between the 2 in the command. The "-r" is an option that modifies the copy command to do something slightly different than usual. The "." means "here", i.e. the destination location is where you currently are.

Now let's see if we can see this data folder we brought in can be "listed".

```bash
ls
```

You should see the string of characters "unix_lesson" show up as the output of `ls`. This is a folder we should all have duplicates of.

> **Tip** - `ls` stands for "list" and it lists the contents of a directory.

## Starting with the shell

Let's go look at what is inside the data folder and explore further. First instead of clicking on the folder name to open it and look at its contents, we have to change the folder we are in. When working with any programming tools, **folders are called directories**. We will be using this terminology moving forward.

To look inside the new unix_lesson directory, we need to change which directory we are in using the command `cd` which stands for "change directory". 

```bash
$ cd unix_lesson
```

Did you notice a change in your command prompt? The "~" symbol from before should have been replaced by the string `unix_lesson`. This means that our `cd` command ran successfully and we are now *in* the new directory. Let's see what is in here by listing the contents:

```bash
$ ls
```

You should see:

```
genomics_data  other  raw_fastq  README.txt  reference_data
```

### Arguments

There are five items listed when you run `ls`, but what types of files are they, or are they directories? 

We can modify the default behavior of `ls` with one or more **"arguments"** to get more information. 

```bash
$ ls -F

genomics_data/  other/  raw_fastq/  README.txt  reference_data/
```

Anything with a "/" after its name is a directory. Things with an asterisk "*" after them are programs.  If there are no "decorations" after the name, it's a normal text file.

You can also use the argument `-l` to show the directory contents in a long-listing format that provides a lot more information:

```bash
$ ls -l
```

```
total 124
drwxrwsr-x 2 mp298 mp298  78 Sep 30 10:47 genomics_data
drwxrwsr-x 6 mp298 mp298 107 Sep 30 10:47 other
drwxrwsr-x 2 mp298 mp298 228 Sep 30 10:47 raw_fastq
-rw-rw-r-- 1 mp298 mp298 377 Sep 30 10:47 README.txt
drwxrwsr-x 2 mp298 mp298 238 Sep 30 10:47 reference_data
```

Each line of output represents a file or a directory. The directory lines start with `d`. If you want to combine the 2 arguments `-l` and `-F`, you can do so by saying the following:

```bash
ls -lF
```

See the modification in the output?

> **Tip** - All commands are essentially programs that are able to perform specific, commonly-used tasks.

Most commands will take additional arguments that control their exact behavior, some of them will take a file or directory name as input. How do we know what the available arguments that go with a particular command are? Most commonly used shell commands have a manual available in the shell. You can access the
manual using the `man` command. Let's try this command with `ls`:

```bash
$ man ls
```

This will open the manual page for `ls` and you will lose the command prompt. It will bring you to a so-called "buffer" page, a page you can navigate with your mouse or if you want to use your keyboard we have listed some basic key strokes:
* 'spacebar' to go forward 
* 'b' to go backward

**To get out of the `man` "buffer" page and to be able to type commands again on the command prompt, press the `q` key!**

***

**Exercise**

* Open up the manual page for the `find` command. Skim through some of the information. 
    * Do you think you might be able to learn this much information about the very many command by heart? 
    * Do you think this format of information display is useful for you?
* Quit the `man` buffer and come back to your command prompt.

> **Tip** - Shell commands can get extremely complicated. No one can possibly learn all of these arguments, of course. So you will probably find yourself referring to the manual page frequently.
>
> **Tip** - If the manual page within the Terminal is hard to read and traverse, the manual exists online too. Use your web searching powers to get it! In addition to the arguments, you can also find good examples online; ***Google is your friend.***

***


## The Unix directory file structure (a.k.a. where am I?)
 
As you've already just seen, you can move around in different directories or folders at the command line. 

### Moving around the file system

Let's practice moving around a bit. Let's go into the raw_fastq directory and see what is in there.

```bash
$ cd raw_fastq/

$ ls -l
```

All six items in this directory look like files ending in `.fq` extension. 

That's all well and good, but where are we in the context of the directory that contains the `unix_lesson` directory?!

Like on any computer you have used before the file structure within unix is hierarchical, like an upside down tree with root (/) as the starting point of the tree-like structure:

<img src="../img/directory_structure.png" width="600">

That root (/) is often also called the 'top' level.

When you log in to a remote computer you are on one of the branches of that tree, your home directory (e.g. /home/username)

> On mac OS, which is a UNIX-based OS, the root level is also "/". On a windows OS, it is drive specific; generally "C:\" is considered root, but it changes to "D:/", if you are on that drive.

Now let's go do that same navigation at the command line.

Type:

```bash
$ cd
```

> This puts you in your home directory. No matter where you are in the directory system, `cd` will always bring you back to your home directory.


Now using `cd` and `ls`, go in to the `unix_lesson` directory and list its contents. Now go into the `raw_fastq` directory, and list its contents.

Let's also check to see where we are. Sometimes when we're wandering around in the file system, it's easy to lose track of where we are. The command that tells you this is:

```bash
$ pwd
```

> This stands for 'print working directory'. i.e. the directory you're currently working in.

What if we want to move back up and out of the `raw_fastq` directory? Can we just type `cd unix_lesson`? Try it and see what happens.

To go 'back up a level' we can use `..`

Type:

```bash
$ cd ..
```

Now do `ls` and `pwd`. 

> `..` denotes parent directory, and you can use it anywhere in the system to go back to the parent directory. Can you think of an example when this won't work?

Finally, there is handy command that can help you see the structure of any directory, namely `tree`.

```bash
#Ensure that you are in your unix_lesson directory and run the following command

$ tree
```

### Examining the contents of other directories

By default, the `ls` commands lists the contents of the working directory (i.e. the directory you are in). You can always find the directory you are in using the `pwd` command. However, you can also give `ls` the names of other directories to view. Navigate to the home directory if you are not already there.

Type:

```bash
$ cd
```

Then enter the command:

```bash
$ ls unix_lesson/
```

This will list the contents of the `unix_lesson` directory without you having to navigate there.

The `cd` command works in a similar way.

```bash
$ cd unix_lesson/raw_fastq/
$ pwd
```

You should now be in `raw_fastq` and you got there without having to go through the intermediate directory. 

> If you are aware of the directory structure, you can string together as long a list as you like.


****

**Exercise**

List the `Mov10_oe_1.subset.fq` file from your home directory without changing directories

****

## Full vs. Relative Paths

The `cd` command takes an argument which is the directory name. Directories can be specified using either a *relative path* or a *full path*. As we know, the directories on the computer are arranged into a hierarchy. The full path tells you where a directory is in that hierarchy. Navigate to the home directory (`cd`). Now, enter the `pwd` command and you should see:

```bash
$ pwd
```

```
/home/username
```

which is the full path for your home directory. This tells you that you are in a directory called `username`, which sits inside a directory called `home` which sits inside the very top directory in the hierarchy, the *root directory*. So, to summarize: `username` is a directory in `home` which is a directory in `/`.

Now enter the following command:

```bash
$ cd /home/username/unix_lesson/raw_fastq/
```

This jumps to `raw_fastq`. Now go back to the home directory (`cd`). We saw
earlier that the command:

```bash
$ cd unix_lesson/raw_fastq/
```

had the same effect - it took us to the `raw_fastq` directory. But, instead of specifying the full path (`/home/username/unix_lesson/raw_fastq`), we specified a *relative path*. In other words, we specified the path **relative to our current working directory**. 

**A full path always starts with a `/`, a relative path does not.**

A relative path is like getting directions from someone on the street. They tell you to "go right at the Stop sign, and then turn left on Main Street". That works great if you're standing there together, but not so well if you're trying to tell someone how to get there from another country. A full path is like GPS coordinates. It tells you exactly where something is no matter where you are right now.

You can usually use either a full path or a relative path depending on what is most convenient. If we are in the home directory, it is more convenient to just enter the relative path since it involves less typing.

Over time, it will become easier for you to keep a mental note of the structure of the directories that you are using and how to quickly navigate among them.

***

**Exercise**

Change directories to `/home/username/unix_lesson/raw_fastq/`, and list the contents of `unix_lesson/other` without changing directories again.

***

### Saving time with tab completion, wildcards and other shortcuts 

#### Tab completion

Navigate to the home directory. Typing out directory names can waste a lot of time. When you start typing out the name of a directory, then hit the tab key, the shell will try to fill in the rest of the directory name. For example, type `cd` to get back to your home directly, then enter:

```bash
$ cd uni<tab>
```

The shell will fill in the rest of the directory name for `unix_lesson`. Now go to `unix_lesson/raw_fastq` and 

```bash
$ ls Mov10_oe_<tab><tab>
```

When you hit the first tab, nothing happens. The reason is that there are multiple directories in the home directory which start with `Mov10_oe_`. Thus, the shell does not know which one to fill in. When you hit tab again, the shell will list the possible choices.

Tab completion can also fill in the names of commands. For example, enter `e<tab><tab>`. You will see the name of every command that starts with an `e`. One of those is `echo`. If you enter `ech<tab>` you will see that tab completion works. 

> **Tab completion is your friend!** It helps prevent spelling mistakes, and speeds up the process of typing in the full command.

#### Wild cards

Navigate to the `~/unix_lesson/raw_fastq` directory. This directory contains FASTQ files from a next-generation sequencing dataset. 

The '*' character is a shortcut for "everything". Thus, if you enter `ls *`, you will see all of the contents of a given directory. Now try this command:

```bash
$ ls *fq
```

This lists every file that ends with a `fq`. This command:

```bash
$ ls /usr/bin/*.sh
```

Lists every file in `/usr/bin` that ends in the characters `.sh`.

```bash
$ ls Mov10*fq
```

lists only the files that begin with 'Mov10' and end with 'fq'

So how does this actually work? The shell (bash) considers an asterisk "*" to be a wildcard character that can be used to substitute for any other single character or a string of characters. 

> An asterisk/star is only one of the many wildcards in UNIX, but this is the most powerful one and we will be using this one the most for our exercises.

****

**Exercise**

Do each of the following using a single `ls` command without
navigating to a different directory.

1.  List all of the files in `/bin` that start with the letter 'c'
2.  List all of the files in `/bin` that contain the letter 'a'
3.  List all of the files in `/bin` that end with the letter 'o'

BONUS: List all of the files in `/bin` that contain the letter 'a' or 'c'.

****


#### Shortcuts

There are some shortcuts which you should know about. Dealing with the
home directory is very common. So, in the shell the tilde character,
"~", is a shortcut for your home directory. Navigate to the `raw_fastq`
directory:

```bash
$ cd
```

```bash
$ cd unix_lesson/raw_fastq
```

Then enter the command:

```bash
$ ls ~
```

This prints the contents of your home directory, without you having to type the full path because the tilde "~" is equivalent to "/home/username".

Another shortcut is the "..":

```bash
$ ls ..
```

The shortcut `..` always refers to the directory above your current directory. So, it prints the contents of the `unix_lesson`. You can chain these together, so:

```bash
$ ls ../..
```

prints the contents of `/home/username` which is your home directory. 

Finally, the special directory `.` always refers to your current directory. So, `ls`, `ls .`, and `ls ././././.` all do the same thing, they print the contents of the current directory. This may seem like a useless shortcut right now, but we used it earlier when we copied over the data to our home directory.


To summarize, while you are in your home directory, the commands `ls ~`, `ls ~/.`, and `ls /home/username` all do exactly the same thing. These shortcuts are not necessary, but they are really convenient!

#### Command History

You can easily access previous commands.  Hit the up arrow. Hit it again.  You can step backwards through your command history. The down arrow takes your forwards in the command history.

'Ctrl-r' will do a reverse-search through your command history.  This
is very useful.

You can also review your recent commands with the `history` command.  Just enter:

```bash
$ history
```

to see a numbered list of recent commands, including this just issues
`history` command. Only a certain number of commands are stored and displayed with `history`, there is a way to modify this to store a different number.

> **NOTE:** So far we have only run very short commands that have few or no arguments, and so it would be faster to just retype it than to check the history. However, as you start to run analyses on the commadn-line you will find your commands to be more complex and the history to be very useful!

**Other handy command-related shortcuts**

- <button>Ctrl + C</button> will cancel the command you are writing, and give you a fresh prompt.
- <button>Ctrl + A</button> will bring you to the start of the command you are writing.
- <button>Ctrl + E</button> will bring you to the end of the command.


## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, search files, redirect. But what if we want to do normal things like copy files or move them around or get rid of them. Sure we could do most of these things without the command line, but what fun would that be?! Besides it's often faster to do it at the command line, or you'll be on a remote server like Amazon where you won't have another option.

Our raw data in this case is fastq files. We don't want to change the original files, so let's make a copy to work with.

Lets copy the file using the copy `cp` command. Navigate to the `raw_fastq` directory and enter:

```bash
$ cp Mov10_oe_1.subset.fq Mov10_oe_1.subset-copy.fq

$ ls -l
```

Now ``Mov10_oe_1.subset-copy.fq`` has been created as a copy of `Mov10_oe_1.subset.fq`

Let's make a 'backup' directory where we can put this file.

The `mkdir` command is used to make a directory. Just enter `mkdir`
followed by a space, then the directory name.

```bash
$ mkdir backup
```

> File/directory/program names with spaces in them do not work in unix, use characters like hyphens or underscores instead.

We can now move our backed up file in to this directory. We can move files around using the command `mv`. Enter this command:

```bash
$ mv *copy.fq backup
```

```bash
$ ls -l backup

-rw-rw-r-- 1 mp298 mp298 75706556 Sep 30 13:56 Mov10_oe_1.subset-copy.fq
```

The `mv` command is also how you rename files. Since this file is so
important, let's rename it:

```bash
$ cd backup

$ mv Mov10_oe_1.subset-copy.fq Mov10_oe_1.subset-backup.fq

$ ls

Mov10_oe_1.subset-backup.fq
```

Finally, we decided this was silly and want to start over.

```bash
$ cd ..

$ rm backup/Mov*
```

> The `rm` file permanently removes the file. Be careful with this command. The shell doesn't
just nicely put the files in the Trash. They're really gone.
>
> Same with moving and renaming files. It will **not** ask you if you are sure that you want to "replace existing file". You can use `rm -i` if you want it to ask before deleting the file(s).

We really don't need these backup directories, so, let's delete both. By default, `rm`, will NOT delete directories, but you use the `-r` flag if you are sure that you want to delete the directories and everything within them. To be safe, let's use it with the `-i` flag.

```bash
$ rm -ri backup/ 
```

- `-r`: recursive, commonly used as an option when working with directories, e.g. with `cp`. 
- `-i`: prompt before every removal.

## Commands, options, and keystrokes covered

```
bash
cd
ls
man
pwd
~           # home dir
.           # current dir
..          # parent dir
*           # wildcard
echo
ctrl + c    # cancel current command
ctrl + a    # start of line
ctrl + e    # end of line
history
cat
less
head
tail
cp
mdkir
mv
rm
```

#### Information on the shell

shell cheat sheets:<br>
* [http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/](http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/)
* [https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md](https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md)

Explain shell - a web site where you can see what the different components of
a shell command are doing.  
* [http://explainshell.com](http://explainshell.com)
* [http://www.commandlinefu.com](http://www.commandlinefu.com)

Data Carpentry tutorial: [Introduction to the Command Line for Genomics](https://datacarpentry.org/shell-genomics/)

General help:
- http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html
- man bash
- Google - if you don't know how to do something, try Googling it. Other people
have probably had the same question.
- Learn by doing. There's no real other way to learn this than by trying it
out.  Write your next paper in vim (really emacs or vi), open pdfs from
the command line, automate something you don't really need to automate.

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*
