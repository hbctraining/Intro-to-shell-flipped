---
title: "Advanced Shell"
author: "Radhika Khetani, Meeta Mistry"
date: "May 9, 2018"
---

## Learning Objectives

* Increasing productivity when working on the command-line and in a cluster environment
* Becoming familiar with advanced bash commands and utilities 

## Advanced Bash Commands and Utilities

As you begin working more with the Shell, you will discover that there are mountains of different utilities at your fingertips to help increase command-line productivity. So far we have introduced you to some of the basics to help get you started. In this lesson, we will touch on some more advanced topics that can be very useful as you carry out analyses in a cluster environment. 

## O2-specific utilities

* [Configuring your shell](#config)
    * [`.bashrc` versus `.bash_profile`](#bashrc)
    * [Aliases](#alias) 
* [Symbolic links](#symlinks)
* [Transferring files with `rsync`](#rsync)
    * [Working on `/n/scratch2/`](#nscratch)

***

## Configuring your shell <a name="config"></a>

In your home directory there are two hidden files `.bashrc` and `.bash_profile`. These files contain all the startup configuration and preferences for your command line interface and are loaded before your Terminal loads the shell environment. Modifying these files allow you to change your preferences for features like your command prompt, the colors of text, and adding aliases for commands you use all the time. 

> **NOTE:** These files begin with a dot (`.`) which makes it a hidden file. To view all hidden files in your home directory you can use:
> 
> `$ ls -al ~/`

### `.bashrc` versus `.bash_profile` <a name="bashrc"></a>

You can put configurations in either file, and you can create either if it doesn’t exist. **But why two different files? What is the difference?**

The difference is that **`.bash_profile` is executed for login shells, while `.bashrc` is executed for interactive non-login shells**. It is helpful to have these separate files when there are preferences you only want to see on the login and not every time you open a new terminal window. For example, suppose you would like to print some lengthy diagnostic information about your machine (load average, memory usage, current users, etc) - the `.bash_profile` would be a good place since you would only want in displayed once when starting out.   

Most of the time you don’t want to maintain two separate configuration files for login and non-login shells. For example, when you export a `$PATH` (as we had done previously), you want it to apply to both. You can do this by sourcing `.bashrc` from within your `.bash_profile` file. Take a look at your `.bash_profile` file, it has already been done for you:

```bash
$ less ~/.bash_profile
```

You should see the following lines:

```bash
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
```

What this means is that if a `.bashrc` files exist, all configuration settings will be sourced upon logging in. Any settings you would like applied to all shell windows (login and interactive) can simply be added directly to the `.bashrc` file rather than in two separate files.


### Aliases <a name="alias"></a>

An alias is a short name that the shell translates into another (usually longer) name or command. They are typically placed in the `.bash_profile` or `.bashrc` startup files so that they are available to all subshells. You can use the `alias` built-in command without any arguments, and the **shell will display a list of all defined aliases**:

```bash
$ alias
```

This should return to you the list of aliases that have been set for you, and you can see **the syntax used for setting an alias is**:

```bash
alias aliasname=value
```

When setting an alias **no spaces are permitted around the equal sign**. If value contains spaces or tabs, you must enclose the value within quotation marks. If you look through the list of aliases that have been set for you, `ll` is a good example of this:

```bash
alias ll='ls -l'
```

Since we have a modifier `-l` and there is a space required, the quotations are necessary.

Let's **setup our own alias**! Every time we want to start an interactive session we have type out this lengthy command. Wouldn't it be great if we could type in a short name instead? Open up the `.bashrc` file using `vim`:

```bash
$ vim ~/.bashrc
```

Scroll down to the heading "`# User specific aliases and functions`" and on the next line you can set your alias:

```bash
o2i='srun --pty -p interactive -t 0-1:00 --mem 1G -c 1 /bin/bash'
```

Save and quit. Now we can source the `.bash_profile` file and test it out. By typing `o2i` at the command prompt we will request an interactive session for 12 hours with 8G of memory. You can change the directives to those you use more often (i.e add more cores, increase memory).


```bash
$ source ~/.bash_profile

$ o2i
```

## Symbolic links <a name="symlinks"></a>

The O2 cluster supports symbolic links also known as symlinks. This is a kind of “file” that is **essentially a pointer to another file name**. Symbolic links can be made to directories or across file systems with no restrictions. You can also make a symbolic link to a name which is not the name of any file. (Opening this link will fail until a file by that name is created.) Likewise, if the symbolic link points to an existing file which is later deleted, the symbolic link continues to point to the same file name even though the name no longer names any file.

Symlinks can be used in lieu of copying over large files. For example, when we began the RNA-seq part of this workshop we had copied over FASTQ files from `~/unix_lesson/raw_fastq` to `~/rnaseq/raw_data`. But what we could have done instead is created symlinks to those files.

The basic syntax for creating a symlink is:

```bash
ln -s /path/to/file /path/to/symlink
```

So if we wanted to have symlinks to our FASTQ files instead of having duplicate copies, we can first remove the files that are currrently there:

```bash
$ rm ~/rnaseq/raw_data/*
```

And then we can symlink the files:

```bash
$ ln -s ~/unix_lesson/raw_fastq/*.fq ~/rnaseq/raw_data/
```

Now, if you check the directory where we created the symlinks you should see the filenames listed in cyan text followed by an arrow pointing the actual file location. (_NOTE: If your files are flashing red text, this is an indication your links are broken so you might want to double check the paths._)

```bash
$ ll ~/rnaseq/raw_data
```

## Transferring files with `rsync` <a name="rsync"></a>

During this workshop we have mostly used Filezilla to transfer files to and from your laptop to the O2 cluster. At the end of the Alignment/Counting lesson we also introduced how to do this on the command line using `scp`. The way `scp` works is it reads the source file and writes it to the destination. It performs a plain linear copy, locally, or over a network.

When **transferring large files or a large number of files `rsync` is a better command** to use. `rsync` employs a special delta transfer algorithm and a few optimizations to make the operation a lot faster. **It will check files sizes and modification timestamps** of both file(s) to be copied and the destination, and skip any further processing if they match. If the destination file(s) already exists, the delta transfer algorithm will **make sure only differences between the two are sent over.**

There are many modifiers for the `rsync` command, but in the examples below we only introduce a select few that we commonly use during a file transfer.

**Example 1:**

```
rsync -t --progress /path/to/transfer/files/*.c /path/to/destination
```

This command would transfer all files matching the pattern *.c from the transfer directory to the destination directory. If any of the files already exist at the destination then the rsync remote-update protocol is used to update the file by sending only the differences.

**Example 2:**

```
rsync -avr --progress /path/to/transfer/directory /path/to/destination
```

This command would recursively transfer all files from the transfer directory into the destination directory. The files are transferred in "archive" mode (`-a`), which ensures that symbolic links, devices, attributes, permissions, ownerships, etc. are preserved in the transfer. In both commands, we have additional modifiers for verbosity so we have an idea of how the transfer is progressing (`-v`, `--progress`)

> **NOTE:** A trailing slash on the transfer directory changes the behavior to avoid creating an additional directory level at the destination.  You can think of a trailing `/ ` as meaning "copy the contents of this directory" as opposed to "copy the  directory  by  name".

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from training materials generated by [FAS Reseach Computing at Harvard University](https://www.rc.fas.harvard.edu/training/training-materials/) and [HMS Research Computing](https://rc.hms.harvard.edu/)*


