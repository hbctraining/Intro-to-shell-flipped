---
title: "Shell scripts and other related concepts"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu"
date: "Thursday, May 5, 2016"
---

Approximate time: 60 minutes

## Learning Objectives

* Capture previous commands into a script to re-run in one single command
* Understanding variables and storing information


Now that you've been introduced to a number of commands to interrogate your data, wouldn't it be great if you could do this for each set of data that comes in, without having to manually re-type the commands?

Welcome to the beauty and purpose of shell scripts.

## Shell scripts

Shell scripts are **text files that contain commands we want to run**. As with any file, you can give a shell script any name and usually have the extension `.sh`. For historical reasons, a bunch of commands saved in a file is usually called a shell script, but make no mistake, this is actually a small program. 


We are finally ready to see what makes the shell such a powerful programming environment. We are going to take the commands we repeat frequently and save them into a file so that we can **re-run all those operations** again later by typing **one single command**. Let's write a shell script that will do two things:

1. Tell us our current working directory
2. List the contents of the directory 

First open a new file using `vim`:

```bash
$ vim listing.sh
```

Then type in the following lines in the `listing.sh` file:

```bash
echo "Your current working directory is:"
pwd
echo "These are the contents of this directory:"
ls -l 
```

Exit `vim` and save the file. Now let's run the new script we have created. To run a shell script you usually use the `bash` or `sh` command.

```bash
$ sh listing.sh
```

> Did it work like you expected?
> 
> Were the `echo` commands helpful in letting you know what came next?

This is a very simple shell script, just to introduce you to the concept. Later in this session, we will be learning how to write more complex ones scripts to illustrate the power of scripts and how they can make our lives (when coding) much easier. Any type of data you will want to analyze will inevitably involve not just one step, but many steps and perhaps many different tools/software programs. Compiling these into a shell script is the first step in creating your analysis workflow!

Before we jump into more scripts, we will take a moment to cover some key concepts to help get you there.

## Bash variables
A *variable* is a common concept shared by many programming languages. Variables are essentially a symbolic/temporary name for, or a reference to, some information. Variables are analogous to "buckets", where information can be stored, maintained and modified without too much hassle. 

Extending the bucket analogy: the bucket has a name associated with it, i.e. the name of the variable, and when referring to the information in the bucket, we use the name of the bucket, and do not directly refer to the actual data stored in it.

Let's start with a simple variable that has a single number stored in it:

```bash
$ num=25
```

*How do we know that we actually created the bash variable?* We can use the `echo` command to print to terminal:

```bash
$ echo num
```

What do you see in the terminal? The `echo` utility takes what arguments you provide and prints to terminal. In this case it interpreted `num` as a a character string and simply printed it back to us. This is because **when trying to retrieve the value stored in the variable, we explicitly use a `$` in front of it**:

```bash
$ echo $num
```

Now you should see the number 25 returned to you. Did you notice that when we created the variable we just typed in the variable name? This is standard shell notation (syntax) for defining and using variables. When defining the variable (i.e. setting the value) you can just type it as is, but when **retrieving the value of a variable don't forget the `$`!** 

Variables can also store a string of character values. In the example below, we define a variable or a 'bucket' called `file`. We will put a filename `Mov10_oe_1.subset.fq` as the value inside the bucket.

```bash
$ file=Mov10_oe_1.subset.fq
```

Once you press return, you should be back at the command prompt. Let's check what's stored inside `file`, but first move into the `raw_fastq` directory::

```bash
$ cd ~/unix_lesson/raw_fastq
$ echo $file
```

Let's try another command using the variable that we have created. We can also count the number of lines in `Mov10_oe_1.subset.fq` by referencing the `file` variable:

```bash
$ wc -l $file
```

> *NOTE:* The variables we create in a session are system-wide, and independent of where you are in the filesystem. This is why we can reference it from any directory. However, it is only available for your current session. If you exit the cluster and login again at a later time, the variables you have created will no longer exist.

***

**Exercise**

* Reuse the `$file` variable to store a different file name, and rerun the commands we ran above (`wc -l`, `echo`)

***

Ok, so we know variables are like buckets, and so far we have seen that bucket filled with a single value. **Variables can store more than just a single value.** They can store multiple values and in this way can be useful to carry out many things at once. Let's create a new variable called `filenames` and this time we will store *all of the filenames* in the `raw_fastq` directory as values. 

To list all the filenames in the directory that have a `.fq` extension, we know the command is:

```bash
$ ls *.fq
```

Now we want to *assign* the output of `ls` as the value of a variable:

```bash
$ allfiles=`ls *.fq`
```

> Note the syntax for assigning output of commands to variables, i.e. the backticks around the `ls` command.

Check and see what's stored inside our newly created variable using `echo`:
	
```bash
$ echo $allfiles
```

Let's try the `wc -l` command again, but this time using our new variable `allfiles` as the argument:

```bash
$ wc -l $allfiles
```

What just happened? Because our variable contains multiple values, the shell runs the command on each value stored in `allfiles` and prints the results to screen. 

***

**Exercise**

* Use some of the other commands we learned in previous lessons (i.e. `head`, `tail`) on the `allfiles` variable. 

***


### The `basename` command

We have one last concept that is a command that will be useful for future scripting. The `basename` command is used for extracting the base name of a file, which is accomplished using **string splitting to strip the directory and any suffix from filenames**. Let's try an example, by first moving back to your home directory:

```bash
$ cd
```

Then we will run the `basename` command on one of the FASTQ files. Be sure to specify the path to the file:

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq
```

What is returned to you? The filename was split into the path `unix_lesson/raw_fastq/` and the filename `Mov10_oe_1.subset.fq`. The command returns only the filename. Now, suppose we wanted to also trim off the file extension (i.e. remove `.fq` leaving only the file *base name*). We can do this by adding a parameter to the command to specify what string of characters we want trimmed.

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq
```

You should now see that only `Mov10_oe_1.subset` is returned. 

***

**Exercise**

* How would you modify the above `basename` command to only return `Mov10_oe_1`?

***


---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


