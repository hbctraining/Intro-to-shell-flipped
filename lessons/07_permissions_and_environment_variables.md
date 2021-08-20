---
title: "Permissions and Environment variables"
author: "Christina Koch, Radhika Khetani, Meeta Mistry, Mary Piper, Jihe Liu"
date: "Thursday, October 8, 2020"
---

Approximate time: 40 minutes

## Learning Objectives
 
* Grant or restrict access to files on a multi-user UNIX system
* View "Environment Variables" in Shell
* Describe $PATH and how to append it

## Permissions

Unix controls who can read, modify, and run files by conferring *permissions* to files and directories. As you can imagine, on a shared system it is important to protect each user's data. This is done by use of **permissions**.

In this lesson, we are going to learn more about how those permissions are set and how they can be modified. 

To start, every file and directory on a Unix computer belongs to one owner (referred to as "user") and one group. Along with each file's content, the operating system stores the information about the user and group that own it, i.e. the "metadata" for a given file. 

**Users of a multi-user Unix system (i.e. the O2 cluster) can belong to any number of groups.**

Let's see what groups we all belong to. Type `groups` into the command prompt.

```bash
$ groups
```

Depending on our affiliation, we all belong to at least a couple of groups. Since we are all using training accounts you will likely see the groups listed below:

* rc_training01 
* training 
* genomebrowser-uploads 
* domain-users

The user-and-group model means that for each file/directory **every user on the system falls into one of three categories**:

* `user` or `u`: **the owner**
* `group` or `g`: **a member of the group the file/directory belongs to**
* `others` or `o`: **everyone else**

For each of these three categories, the computer keeps track of whether people in that category can read the file (`r`), write to the file (`w`), or execute the file (i.e., run the program written in it) (`x`). More about this aspect of permissions is coming up later in this lesson.

Let's look at this model in action by running the command `ls -l /n/groups/hbctraining/`, to list the files in that directory:

```bash
$ ls -l /n/groups/hbctraining/
total 30G
drwxrwsr-x  4 mm573 hbctraining 831 Feb 29  2016 bcbio-rnaseq
drwxrwsr-x 14 mm573 hbctraining 382 Jul 10 17:03 chip-seq
-rw-r--r--  1 root  hbctraining   0 Apr  5  2015 copy_me.txt
drwxrwsr-x  3 rsk27 hbctraining 201 Apr  5  2015 exercises
drwxrwsr-x  6 rsk27 hbctraining 293 Oct 27  2017 for_chipseq
drwxrwsr-x 14 mm573 hbctraining 494 May 21  2018 intro_rnaseq_hpc
.
.
.
.
```

As we have learned, the `-l` flag tells `ls` to give us a long-form listing. Let's take another look at the columns in this output, **starting from the right side moving left**.

1. File/directory names
2. Times and dates last modified. Backup systems and other tools use this information in a variety of ways, but you can use it to tell when you (or anyone else with permission) last changed a file.
3. File size in bytes.
4. Name of the group that owns the file.
5. User name of the file’s owner.
6. File’s number of hard links (not important for this class).
7. Permissions to the file

> *NOTE:* When listing the contents of a directory using the `ls -l` command, the file size reported for directories do not reflect the size of the data inside it. The number actually represents the size of space on the disk that is used to store the metadata for the directory. 
>
> The command you’ll want to use to get the size of a directory's contents is `du -sh`. `du` is short for “*d*isk *u*sage”.

Let's list the contents of the `unix_lesson` directory:

```bash
ls -l ~/unix_lesson/

drwxrwxr-x 2 rc_training01 rc_training01  78 Oct  6 10:57 genomics_data
drwxrwxr-x 2 rc_training01 rc_training01  73 Oct  6 10:57 other
drwxrwxr-x 5 rc_training01 rc_training01 302 Oct  6 11:53 raw_fastq
-rw-rw-r-- 1 rc_training01 rc_training01 377 Oct  6 10:57 README.txt
drwxrwxr-x 2 rc_training01 rc_training01  62 Oct  6 10:57 reference_data
```

Who is the owner of the files in this directory? Which group do the files belong to?

Basically, O2 has you (your account ID) listed both as an owner and a group, and this is usually the assignment for the files and folders in your personal directory. Essentially, when a new user is created on a Unix system, a group of the same name is created and personal files for that user are also "owned" by that user's group.

### Interpreting the permissions string

Let's have a closer look at one of those permission strings in the first column for the `README.txt` file:

```bash
-rw-rw-r--
```

* The first character indicates the type of file. Among the different types, a leading dash (`-`) means a regular file, while a `d` indicates a directory. 

>> In our case, it is `-` which means README.txt is a regular file.

* The next 9 characters are usually some combination of `r`, `w` and `x`, where:

<kbd>r</kbd> = read permission

<kbd>w</kbd> = write/edit permission
 
<kbd>x</kbd> = execute permission (run a script/program or traverse a directory).

The **first triplet** is the permissions for the file’s **owner (`u`)**. Here, the owner can read and write the file: `rw-`. If the permission is turned off, we see a dash, so `rw-` means "read and write, but not execute". 

```bash
rw-
```

The **second triplet** shows us the **group's permissions (`g`)**. Here, the group can read and write the file. (In this case the group and the owner are the same so it makes sense that this is the same for both.)

```bash
rw-
```

The **final triplet** shows us what **everyone else (`o`)** can do. In this case, it's `r--`, so **everyone else** on the system can only read the file's contents. 

> "Everyone" else refers to other users on the system who are not the file's owner, or in the group that the file's belongs to. 
 
```bash
r--
```

> #### The execute permissions
> We don't see the execute permission set here since we are not working with executable files. To see an example of a file that is actually executable, try `ls -l /bin/ls`.
> 
> Sometimes the `x` is replaced by another character, but it is beyond the scope of today's class. You can [get more information here](https://en.wikipedia.org/wiki/File_system_permissions#Notation_of_traditional_Unix_permissions), if you are interested.
>

#### Is the permissions string interpreted in the same way for directories?

If we take a look at the permissions for directories (e.g. `drwxrwsr-x`): the `x` for the permissions here indicates that "execute" is turned on. What does that mean, given that a directory isn't a program or an executable file, we can't "execute" it? 

Well, `x` means something different for directories. It gives someone the right to *traverse* the directory, but not to look at (or list) its contents. This is beyond the scope of today's class, but note that you can give someone access to a file that's deep inside a directory structure *without allowing them to see what other files exist in the sub-directories which are part of the path*.

### Changing permissions

To change permissions, we use the `chmod` command (whose name stands for "change mode"). The arguments we provide `chmod` include:

* Whose permissions are we changing? ("user" <kbd>u</kbd>, "group" <kbd>g</kbd>, or "other" <kbd>o</kbd>)
* Are we adding permissions (<kbd>+</kbd>) or removing permissions (<kbd>-</kbd>)?
* Which permissions (or combination of) would we like to add/remove? ("read" <kbd>r</kbd>, "write" <kbd>w</kbd>, and "execute" <kbd>x</kbd>)

Let's make our README.txt file **inaccessible** to all users other than you and the group the file belong to. Currently, everyone else is able to read the file.

```bash
$ ls -l ~/unix_lesson/README.txt

-rw-rw-r-- 1 rc_training01 rc_training01 377 Oct  6 10:57 ~/unix_lesson/README.txt
```

```bash
$ chmod o-r ~/unix_lesson/README.txt         # the "-" after o denotes removing that permission

$ ls -l ~/unix_lesson/README.txt

-rw-rw---- 1 rc_training01 rc_training01 377 Oct  6 10:57 ~/unix_lesson/README.txt
```

The `o` signals that we're changing the privileges of "others" which also represents "everyone else" as we have referred to throughout this lesson.

Let's change it back to allow it to be readable by others:

```bash
$ chmod o+r ~/unix_lesson/README.txt         # the "+" after o denotes adding/giving that permission

$ ls -l ~/unix_lesson/README.txt

-rw-rw-r-- 1 rc_training01 rc_training01 377 Oct  6 10:57 /home/rsk27/unix_lesson/README.txt
```

If we wanted to make this an executable file for ourselves (the file's owners) we would say `chmod u+x`, where the `u` signals that we are changing permission for the file's owner. To change permissions for the "group", you'd use the letter `g`, e.g. remove write permissions for the group with `chmod g-w`. 

> The fact that something is marked as executable doesn't actually mean it contains or is a program of some kind. We could easily mark the `~/unix_lesson/raw_fastq/Irrel_kd_1.subset.fq` file as executable using `chmod`. Depending on the operating system we're using, trying to "run" it will fail (because it doesn't contain instructions the computer recognizes, i.e. it is not a script of some type).

****

**Exercise**

If `ls -l myfile.php` returns the following details:

```bash
-rwxr-xr-- 1 caro zoo  2312  2014-10-25 18:30 myfile.php
```
 
Which of the following statements is true?
 
1. members of caro (a group) can read, write, and execute myfile.php
2. members of zoo (a group) cannot execute myfile.php
3. caro (the owner) can read, write, and execute myfile.php

     <details>
       <summary><b><i>Click here for the answer</i></b></summary>
       The third statement is true.
     </details>

****

## Environment Variables

Environment variables are, in short, variables that describe the environment in which programs run, and they are predefined for a given computer or cluster that you are on. You can reset them to customize the environment. 

Let's see the full list of environment variables on O2:

```bash
$ env
```

It's a pretty long list! **In the context of the shell the environment variables are usually all in upper case.**

In this lesson, we are going to focus on two most commonly encountered environment variables: `$HOME` and `$PATH`.

* `$HOME` defines the full path for the home directory of a given user.
* `$PATH` defines a list of directories to search in when looking for a command/program to execute.

Environment variables, in most systems, are called or denoted with a "$" before the variable name, just like a regular variable. Let's use the `echo` command to see what is stored in `$HOME`:

```bash
$ echo $HOME
```

You should see the path to your home directory. `$HOME` can be used instead of the `~` (if you want to type 4 more characters).


`$HOME` is pretty straightforward, how about we take a look at what is stored in the `$PATH` variable:

```bash
$ echo $PATH

/n/cluster/bin:/opt/singularity/bin:/usr/local/rvm/gems/ruby-2.4.9/bin:/usr/local/rvm/gems/ruby-2.4.9@global/bin:/usr/local/rvm/rubies/ruby-2.4.9/bin:/n/cluster/bin:/opt/singularity/bin:/usr/local/bin:/usr/bin:/opt/puppetlabs/bin:/usr/local/rvm/bin:/usr/local/sbin:/usr/sbin:/home/rc_training01/.local/bin:/home/rc_training01/bin
```
This output is a lot more complex! Let's break it down. When you look closely at the output of `echo $PATH`, you should a list of full paths separated from each other by a ":". 

Here is the list of paths in a more readable format:
* `/n/cluster/bin`
* `/opt/singularity/bin`
* `/usr/local/rvm/gems/ruby-2.4.9/bin`
* `/usr/local/rvm/gems/ruby-2.4.9@global/bin`
* `/usr/local/rvm/rubies/ruby-2.4.9/bin`
* `/n/cluster/bin:/opt/singularity/bin`
* `/usr/local/bin`
* `/usr/bin`
* `/opt/puppetlabs/bin`
* `/usr/local/rvm/bin`
* `/usr/local/sbin/usr/sbin`
* `/home/rc_training01/.local/bin`
* `/home/rc_training01/bin`

Each of these paths are referring to a directory, in this case a lot of them are named `bin`. 

### What are all these paths? And what do they represent?

These are the directories that the shell will look through (in the same order as they are listed) for any given command or executable file that you type on the command prompt.

For example, we have been using the `ls` command to list contents in a directory. When we type `ls` at the command prompt, the shell searches through each path in `$PATH` until it finds an executable file called `ls`. So which of those paths contain that executable file?

For any command you execute on the command prompt, you can find out where the executable file is located using the `which` command.

```bash
$ which ls
```

What path was returned to you? Does it match with any of the paths stored in `$PATH`?


Try it on a few of the basic commands we have learned so far:
```bash
$ which <your favorite command>
$ which <your favorite command>
```

Check the path `/usr/bin/` and see what other executable files you recognize. (Note that executable files will be listed as green text or have the `*` after their name).

```bash
$ ls -lF /usr/bin/
```

The path `/usr/bin` is usually where executables for commonly used commands are stored. 

> As pointed out earlier, a lot of the folders listed in the `$PATH` variable are called `bin`. This is because of a convention in Unix to call directories that contain all the commands (in ***binary*** format) **`bin`**.

***

**Exercise**

Are the directories listed by the `which` command within `$PATH`?

   <details>
        <summary><b><i>Click here for the answer</i></b></summary>
        It should be. For example, if you would like to check the directory of command <code>pwd</code> - the output for <code>which pwd</code> is <code>/usr/bin/pwd</code>, and <code>/usr/bin</code> is within $PATH.
    </details>

***

#### Modifying Environment Variables

You can modify the contents of the `$PATH` environment variable with the `export` command. 

The `export` command:
* Example `export PATH=$PATH:~/opt/bin` (**do not run this**)
* The arguments or **input to `export` should always include `$PATH`**
  * This specifies that you want to maintain the existing contents.
  * If you don't maintain all the `bin` directories, none of your commands will work anymore!
* Use the ":" to separate added paths from one another, **with no spaces**
* The new path being added, should not end in `/`. Even though `ls ~/opt/bin` and `ls ~/opt/bin/` give you the same results, the `$PATH` variable cannot have the trailing `/`.
* Order matters -
  * If you run `export PATH=$PATH:~/opt/bin` Shell will add the `~/opt/bin` directory to **the end of the pre-existing list** within the `$PATH` environment variable. 
  * Alternatively, if you use `export PATH=~/opt/bin:$PATH`, the same directory will be added to the beginning of the list. The order determines which directory Shell will look in first to find a program.
  
This command is often used to add paths to a directory with commands you commonly want to use. 

Let's say you often use the `bowtie2` command for alignment and it exists in `/home/rsk27/installations/alignment_tools/dna/bowtie/`. 

If you want to run this tool, you will have to type:

```bash 
$ /home/rsk27/installations/alignment_tools/dna/bowtie/bowtie2 <inputfile>
```

However, if `/home/rsk27/installations/alignment_tools/dna/bowtie` is part of the `$PATH` variable you can instead just type:

```bash
$ bowtie2 <inputfile>
```

#### Closer look at the inner workings of the shell, in the context of $PATH
 
Each time you log in to a cluster, or start a new interactive session on a compute node, 2 special shell scripts are run automatically in the background. You have the ability to modify these scripts, so if you want to customize your environment you can add the customizing commands to these shell scripts. One common use of this is to add an `export` command that adds paths to the pre-existing contents of the `$PATH` environment variable.

So, what are these files and where are they located? These are called `.bashrc` and `.bash_profile`, and they are located in your home directory. You can create them if they don't exist, and Shell will use them!


Check what hidden files exist in our home directory using the `-a` flag with the `ls` command:

```bash
$ ls -al ~/
```

> You can use `vim` to modify these files and/or create them.

**In closing, permissions and environment variables, especially `$PATH`, are very useful and important concepts to understand in the context of UNIX and HPC.**

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*

