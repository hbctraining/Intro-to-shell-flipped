---
title: "The Shell"
author: "Sheldon  McKay, Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu"
date: "September 28, 2020"
---

## Learning Objectives
- To access the shell
- To execute basic tasks in the shell, including:
  - Navigating around the Unix file system
  - Differentiating between full and relative paths
  - Listing files in a directory
  - Copying, removing and moving files 


## Setting up

We will spend most of our time learning about the basics of the shell command-line interface (CLI) by exploring experimental data on the **O2** cluster. So, we will need to log in to this remote compute cluster first before we can start with the basics.

Let's take a quick look at the basic architecture of a cluster environment and some cluster-specific jargon prior to logging in.

<p align="center">
<img src="../img/compute_cluster.png" width="500">
</p>

The above image reflects the many computers that make up a **"cluster"** of computers. Each individual computer in the cluster is usually a lot more powerful than any laptop or desktop computer we are used to working with, and is referred to as a **"node"** (instead of computer). Each node has a designated role, either for logging in or for performing computational analysis/work. **A given cluster will usually have a few login nodes and several compute nodes.**

The data on a cluster is also stored differently than what we are used to with our laptops and desktops, in that it is not computer- or node-specific storage, but all of the data is available to all the nodes in a cluster. This ensures that you don't have to worry about which node is working on your analysis. ***We will be going into more depth about the cluster architecture, storage systems and best practices on the last day of this workshop.***

### Logging in to O2

#### O2 accounts

For this workshop we will be using training accounts to log in. These have been created for us by the [HMS Research Computing](https://rc.hms.harvard.edu/) (HMS-RC) team, they are the folks that manage the O2 cluster. We will be providing each of you with your own *training* account associated with a password for the duration of this workshop.

> If you are interested in getting your own personal account on O2, please follow the instructions [provided here](https://wiki.rc.hms.harvard.edu/display/O2/Frequently+Asked+Questions+and+Answers#FrequentlyAskedQuestionsandAnswers-Accountsandloggingin) after this workshop.

#### Tool(s) to access remote computers/clusters

**With Mac OS**

Macs have a utility application called "**Terminal**" for performing tasks on the command line (shell), both locally and on remote machines. We will be using it to log into O2. 

Please find and open the Terminal utility on your computers using the *Spotlight Search* at the top right hand corner of your screen.

**With Windows OS**

By default, there is no built-in Terminal that uses the bash shell on the Windows OS. So, we will be using a downloaded program called "**Git BASH**" which is part of the [Git for Windows](https://git-for-windows.github.io/) tool set. **Git BASH is a shell/bash emulator.** What this means is that it shows you a very similar interface to, and provides you the functionality of, the Terminal utility found on the Mac and Linux Operating systems.

Please find and open Git BASH.

> **Tip** - Windows users can use another program called [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) instead of a *bash emulator* to log in to remote machines, but it is a little more involved and has different capabilities. We encourage you to take a look at it, but we will not be covering it in this workshop.

#### Let's log in! 

Everyone should have their Terminal (or Git BASH Terminal) window open. Using this Terminal window, you can interact with your own computer using bash commands! You see the "$" symbol? That is where you write the "commands" that will be executed by shell (bash in this case) and your computer's kernel. The "$" is called the **"command prompt"**. 

To connect to the login node on O2:

1. Type in the `ssh` command at the command prompt followed by a space, and then type your username (e.g. rc_training10) plus the address of the cluster `@o2.hms.harvard.edu`. There is no space between the username and the "@" symbol (see below).

```bash
ssh username@o2.hms.harvard.edu
```

2. Press the return/enter key and you should receive a prompt for your password. 
3. Type in your password and note that **the cursor will not move as you type** it in! This is normal and know that the computer is receiving and transmitting your typed password to the remote system, i.e. the O2 cluster.
4. If this is the first time you are connecting to the cluster, **a warning will pop up** and will ask you if you are sure you want to do this; **type `Yes` or `Y`**. 

> **Tip** - Syntax for all commands on the command-line interface is the command followed by space and then optionally a few arguments.

Once logged in, you should see the O2 icon, some news, and a new command prompt: 

```bash
[username@login01 ~]$ 
```

The command prompt on O2 will have some characters before the `$`, something like `[rc_training10@login01 ~]`, this is telling you your username and the name of the login node you have connected to!

***Please note that from this point on in the workshop anything we want you to type next to the command prompt will be preceded by the `$` (see below). Please make sure you do not type out (or copy and paste) the `$` at the beginning of a command into the Terminal.***

#### Let's move from the login node to a compute node!

The first command we will run at the command prompt will be to start a so-called "interactive session" on O2. This command will connect us to a compute node, so that all of the commands we run will be processed by a computer designated to do analysis (and not designated to log in users). **Copy and paste the command below.**

```bash
$ srun --pty -p interactive -t 0-2:30 --mem 1G /bin/bash
```
 
Press enter, you should see a couple of messages and in a few seconds you should get back the command prompt `$`. Now the string of characters before the command prompt will be different. They should say something like `[rc_training10@compute-a-16-73 ~]`. This is telling you that you are using one of the compute nodes/computer on the cluster now and it is specifying the name of that compute node.

> **Tip** - When you run any command in Shell, the command prompt might disappear and once command has been executed, the prompt is returned. This indicates that Shell is ready to accept another command.

Make sure that your command prompt now contains the word "compute". Once it does, we are ready to copy over some data to work with!

### Copying example data folder

Now that we are all set up to use O2, the first thing to do is to check if there are any files in the data folder we are currently in. When you log in to a cluster, you will land within a folder designated specifically for your use, and is referred to as your "home directory".

Let's list the contents of our home directory using a command called `ls`.

```bash
$ ls
```

It should show you that you have 0 files, or not show you anything at all because you don't have any data there as yet!

Let's bring in a data folder from a different location on the cluster to our designated area by using the `cp` command. **Copy and paste the following command** all the way from `cp` and including the period symbol at the end `.`:

```bash
$ cp -r /n/groups/hbctraining/unix_lesson/ .
```

>'cp' is the command for copy. This command required you to specify the location of the item you want to copy (/n/groups/hbctraining/unix_lesson/) and the location of the destination (.); please note the space between the 2 in the command. The "-r" is an option that modifies the copy command to do something slightly different than usual. The "." means "here", i.e. the destination location is where you currently are.

Now let's see if we can see this data folder we brought in and if it can be "listed".

```bash
ls
```

You should see the string of characters "unix_lesson" show up as the output of `ls`. This is a folder we should all have duplicates of.

> **Tip** - `ls` stands for "**l**i**s**t" and it lists the contents of a directory.

## Starting with the shell

Let's look at what is inside the data folder and explore further. First instead of clicking on the folder name to open it and look at its contents, we have to change the folder we are in. When working with any programming tools, **folders are called directories**. We will be using folder and directory interchangeably moving forward.

To look inside the `unix_lesson` directory, we need to change which directory we are *in*. To do this we can use the `cd` command, which stands for "change directory". 

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

There are five items listed when you run `ls`, but what types of files are they, or are they directories or files? 

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

Do you see the modification in the output?

<details>
  <summary><i>Explanation</i></summary>
  <P>Notice that the listed directories now have <code>/</code> at the end of their names.</P>
</details>

> **Tip** - **All commands are essentially programs** that are able to perform specific, commonly-used tasks.

Most commands will take additional arguments that control their behavior, some of them will take a file or directory name as input. How do we know what the available arguments that go with a particular command are? Most commonly used shell commands have a manual available in the shell. You can access the
manual using the `man` command. Let's try this command with `ls`:

```bash
$ man ls
```

This will open the manual page for `ls` and you will lose the command prompt. It will bring you to a so-called "buffer" page, a page you can navigate with your mouse or if you want to use your keyboard we have listed some basic key strokes:
* 'spacebar' to go forward 
* 'b' to go backward
* Up or down arrows to go forward or backward, respectively

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

Let's practice moving around a bit. Let's go into the raw_fastq directory and see what is in there.

```bash
$ cd raw_fastq/

$ ls -l
```

Great, we have now traversed some sub-directories, but where are we in the context of our pre-designated "home" directory that contains the `unix_lesson` directory?!

### The "root" directory!

Like on any computer you have used before the file structure within a Unix/Linux system is hierarchical, like an upside down tree with the "/" directory, called "root" as the starting point of this tree-like structure:

<p align="center">
<img src="../img/directory_structure.png" width="600">
</p>

> **Tip** - Yes, the root folder's actual name is just **`/`** (a forward slash).

That `/` or root is the 'top' level.

When you log in to a remote computer you land on one of the branches of that tree, i.e. your pre-designated "home" directory that usually has your login name as its name (e.g. `/home/rsk27`).

> **Tip** - On mac OS, which is a UNIX-based OS, the root level is also "/". 
>
> **Tip** - On a windows OS, it is drive specific; "C:\" is considered the default root, but it changes to "D:/", if you are on that drive.

### Paths 

Now let's learn more about the "addresses" of directories, called **"path"** and move around the file system.

Let's check to see what directory we are in. The command prompt tells us which directory we are in, but it doesn't give information about where the `raw_fastq` directory is with respect to our "home" directory or the `/` directory.

The command to check our current location is `pwd`, this command does not take any arguments and it returns the path or address of your **p**resent **w**orking **d**irectory (the folder you are in currently).

```bash
$ pwd
```

In the output here, each folder is separated from its "parent" or "child" folder by a "/", and the output starts with the root `/` directory. So, you are now able to determine the location of `raw_fastq` directory relative to the root directory!

But which is your pre-designated home folder? No matter where you have navigated to in the file system, just typing in `cd` will bring you to your home directory. 

```bash
$ cd
```

What is your present working directory now?

```bash
$ pwd
```

This should now display a shorter string of directories starting with root. This is the full address to your home directory, also referred to as "**full path**". **The "full" here refers to the fact that the path starts with the root, which means you know which branch of the tree you are on in reference to the root.**

Take a look at your command prompt now, does it show you the name of this directory (your username?)? 

*No, it doesn't. Instead of the directory name it shows you a `~`.*

Why is this so? 

*This is because `~` = full path to home directory for the user.*

Can we just type `~` instead of `/home/username`?

*Yes, we can!*

#### Using paths with commands

You can do a lot more with the idea of stringing together *parent/child* directories. Let's say we want to look at the contents of the `raw_fastq` folder, but do it from our current directory (the home directory. We can use the list command and follow it up with the path to the folder we want to list!

```bash
$ cd

$ ls -l ~/unix_lesson/raw_fastq
```

Now, what if we wanted to change directories from `~` (home) to `raw_fastq` in a single step?

```bash
$ cd ~/unix_lesson/raw_fastq
```

Voila! You have moved 2 levels of directories in one command.

What if we want to move back up and out of the `raw_fastq` directory? Can we just type `cd unix_lesson`? Try it and see what happens.

*Unfortunately, that won't work because when you say `cd unix_lesson`, shell is looking for a folder called `unix_lesson` within your current directory, i.e. `raw_fastq`.*

Can you think of an alternative? 

*You can use the full path to unix_lesson!*

```bash
$ cd ~/unix_lesson
```

****

**Exercises**

1. Using one command move to your home directory.
2. Using one command list the contents of the `reference_data` directory that is within the `unix_lesson` directory.
3. Using one command list one of the files in `reference_data`.

****

#### Tab completion

Typing out full directory names can be time-consuming and error-prone. One way to avoid that is to use **tab completion**. The `tab` key is located on the left side of your keyboard, right above the `caps lock` key. When you start typing out the first few characters of a directory name, then hit the `tab` key, Shell will try to fill in the rest of the directory name. 

For example, first type `cd` to get back to your home directly, then type `cd uni`, followed by pressing the `tab` key:

```bash
$ cd
$ cd uni<tab>
```

The shell will fill in the rest of the directory name for `unix_lesson`. 

Now, let's go into `raw_fastq`, then type `ls Mov10_oe_`, followed by pressing the `tab` key once:

```bash
$ cd raw_fastq/
$ ls Mov10_oe_<tab>
```

**Nothing happens!!**

The reason is that there are multiple files in the `raw_fastq` directory that start with `Mov10_oe_`. As a result, shell does not know which one to fill in. When you hit `tab` a second time again, the shell will then list all the possible choices.

```bash
$ ls Mov10_oe_<tab><tab>
```

Now you can select the one you are interested in listed, and enter the number and hit tab again to fill in the complete name of the file.

```bash
$ ls Mov10_oe_1<tab>
```

> **NOTE:** Tab completion can also fill in the names of commands. For example, enter `e<tab><tab>`. You will see the name of every command that starts with an `e`. One of those is `echo`. If you enter `ech<tab>`, you will see that tab completion works. 

**Tab completion is your friend!** It helps prevent spelling mistakes, and speeds up the process of typing in the full command. We encourage you to use this when working on the command line. 

#### Relative paths

We have talked about **full** paths so far, but there is a way to specify paths to folders and files without having to worry about the root directory. And you have used this before when we were learning about the `cd` command.

Let's change directories back to our home directory, and once more change directories from `~` (home) to `raw_fastq` in a single step. (*Feel free to use your tab-completion to complete your path!*)

```bash
$ cd
$ cd unix_lesson/raw_fastq
```

This time we are not using the `~/` before `unix_lesson`. In this case we are using a relative path, relative to our current location - wherein we know that `unix_lesson` is a child folder in our home folder, and the `raw_fastq` folder is within `unix_lesson`.

> Previously we had used the following:
> ```bash
> $ cd ~/unix_lesson/raw_fastq
> ```

There is also a handy shortcut for the relative path to a parent directory, 2 periods `..`. Let's say we wanted to move from the `raw_fastq` folder to its parent folder.

```bash
cd ..
```

You should now be in the `unix_lesson` directory (check command prompt or run `pwd`).

> You will be learning a little more about the `..` shortcut later. Can you think of an example when this shortcut to the parent directory won't work?
>
>   <details>
>     <summary>Answer</summary>
>     <P>When you are at the root directory, since there is no parent to the root directory!</P>
>   </details>

When using relative paths, you might need to check what the branches are downstream of the folder you are in. There is a really handy command (`tree`) that can help you see the structure of any directory.

```bash
$ tree
```

If you are aware of the directory structure, you can string together as long a list of directories as you like using either **relative** or **full** paths.

#### Synopsis of Full versus Relative paths

**A full path always starts with a `/`, a relative path does not.**

A relative path is like getting directions from someone on the street. They tell you to "go right at the Stop sign, and then turn left on Main Street". That works great if you're standing there together, but not so well if you're trying to tell someone how to get there from another country. A full path is like GPS coordinates. It tells you exactly where something is no matter where you are right now.

You can usually use either a full path or a relative path depending on what is most convenient. If we are in the home directory, it is more convenient to just enter the relative path since it involves less typing.

Over time, it will become easier for you to keep a mental note of the structure of the directories that you are using and how to quickly navigate among them.


## Copying, creating, moving and removing data

Now we can move around within the directory structure using the command line. But what if we want to do things like copy files or move them from one directory to another, rename them? 

Let's move into the `raw_fastq` directory, this contains some fastq files which are the output of sequencing. 

```bash
cd ~/unix_lesson/raw_fastq
```

> **Tip** - These files are referred to as "raw" data since it has not been changed or analyzed after being generated.

### Copying

Let's use the copy (`cp`) command to make a copy of one of the files in this folder, `Mov10_oe_1.subset.fq`, and call the copied file `Mov10_oe_1.subset-copy.fq`. 
The copy command has the following syntax: 

`cp  path/to/item-being-copied  path/to/new-copied-item`

In this case the files are in our current directory, so we just have to specify the name of the file being copied, followed by whatever we want to call the newly copied file.

```bash
$ cp  Mov10_oe_1.subset.fq  Mov10_oe_1.subset-copy.fq

$ ls -l
```

The copy command can also be used for copying over whole directories, but the `-r` argument has to be added after the `cp` command. The `-r` stands for recursively copy everything from the directory and its sub-directories". [We used it earlier when we copied over the `unix_lesson` directory to our home directories]().

### Creating

Next, let's create a directory called `fastq_backup` and we can move the copy of the fastq file into that directory. 

The `mkdir` command is used to make a directory, syntax: `mkdir  name-of-folder-to-be-created`.

```bash
$ mkdir fastq_backup
```

> **Tip** - File/directory/program names with spaces in them do not work well in Unix, use characters like hyphens or underscores instead. Using underscores instead of spaces is called "snake_case". Alternatively, some people choose to skip spaces and rather just capitalize the first letter of each new word (i.e. MyNewFile). This alternative technique is called "CamelCase".

### Moving

We can now move our copied fastq file in to the new directory. We can move files around using the move command, `mv`, syntax: 

`mv  path/to/item-being-moved  path/to/destination` 

In this case we can use relative paths and just type the name of the file and folder.

```bash
$ mv  Mov10_oe_1.subset-copy.fq  fastq_backup
```

Let's check if the move command worked like we wanted:

```bash
$ ls -l fastq_backup
```

### Renaming

The `mv` command has a second functionality, it is what you would use to rename files too. The syntax is identical to when we used `mv` for moving, but this time instead of giving a directory as its destination, we just give a new name as its destination. 

Let's try out this functionality!

The name Mov10_oe_1.subset-copy.fq is not very informative, we want to make sure that we have the word "backup" in it so we don't accidentally delete it.

```bash
$ cd fastq_backup

$ mv  Mov10_oe_1.subset-copy.fq   Mov10_oe_1.subset-backup.fq

$ ls
```

> **Tip** - You can use move to move a file and rename it at the same time!

**Important notes about `mv`**:
* When using `mv`, shell will **not** ask if you are sure that you want to "replace existing file" or similar unless you use the -i option. 
* Once replaced, it is not possible to get the replaced file back!

### Removing

We find out that we did not need to create backups of our fastq files manually as backups were generated by our collaborator; in the interest of saving space on the cluster we want to delete the contents of the `fastq-backup` folder and the folder itself. 

```bash
$ rm  Mov10_oe_1.subset-backup.fq
```

Important notes about `rm`
* `rm` permanently removes/deletes the file/folder. 
* There is no concept of "Trash" or "Recycle Bin" on the command-line. When you use `rm` to remove/delete they're really gone. 
* **Be careful with this command!**
* You can use the `-i` argument if you want it to ask before removing, `rm -i file-name`.

Let's delete the fastq_backup folder too. First, we'll have to navigate our way to the parent directory (we can't delete the folder we are currently in/using). 

```bash
$ cd ..

$ rm  fastq_backup 
```

Did that work? Did you get an error?

<details>
  <summary><i>Explanation</i></summary>
  <P>By default, <code>rm</code>, will NOT delete directories, but you use the <code>-r</code> flag if you are sure that you want to delete the directories and everything within them. To be safe, let's use it with the <code>-i</code> flag.</P>
</details><br>

```bash
$ rm -ri fastq_backup
```

- `-r`: recursive, commonly used as an option when working with directories, e.g. with `cp`. 
- `-i`: prompt before every removal.

***

**Exercise**

1. Create a new folder in `unix_lesson` called `selected_fastq`
2. Copy over the Irrel_kd_2.subset.fq and Mov10_oe_2.subset.fq from `raw_fastq` to the `~/unix_lesson/selected_fastq` folder
3. Rename the `selected_fastq` folder and call it `exercise1`

***

## Commands

```
cd          # change directory
ls          # list contents
man         # manual for a command
pwd         # check present working directory
tree        # prints a tree of the file structure
cp          # copy
mdkir       # make new directory
mv          # move or rename 
rm          # remove/delete
```

## Shortcuts

```
~           # home directory
.           # current directory
..          # parent directory
```

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*
