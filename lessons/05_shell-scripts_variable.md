---
title: "Shell scripts and other related concepts"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu"
date: "Thursday, May 5, 2016"
---

Approximate time: 60 minutes

## Learning Objectives

* Capture previous commands into a shell script
* Understanding variables and their use case
* Extracting useful text from a filename to create a prefix


## Shell scripts

By this point on the workshop you have been introduced to a number of commands to interrogate your data. To demonstrate the function of each we have run the commands one at a time at the command prompt. The command prompt is useful for testing out commands and also performing simple tasks like exploring and organizing the filesystem. When we are running analyses which require a series of tasks to be run and sometimes repeat these tasks on multiple files, there is a more efficent way to do this using shell scripts. 

Shell scripts are **text files that contain commands we want to run**. In this lesson we will introduce you to shell scripts by providing a simple example one. We will then introduce a few related concepts which will in turn help us to create a more advanced shell script example.

### A simple script

We are finally ready to see what makes the shell such a powerful programming environment. To create our first script, we are going to take some of the commands we have run previously and save them into a file so that we can **re-run all those operations** again later by typing **one single command**. For historical reasons, a bunch of commands saved in a file is referred to as shell script, but make no mistake, this is actually a small program. 

As with any file in bash, you can give a shell script any name (with or without an extension). However, it is best practice and helpful to you and your collaborators to easily identify a file as a shell script if it has the extension `.sh`. 

Move over to the `other` directory and create a new file using `vim`. We will call our script `listing.sh`:

```bash
$ cd ~/unix_lesson/other
$ vim listing.sh
```

This shell script will do two things:

1. Tell us our current working directory
2. List the contents of the directory 

We already know the commands for doing both of these things, so let's add them into our script:

```bash
pwd
ls -l 
```

Now, we could save and quit and this shell script would run perfectly fine. But instead, we will add some verbosity to our script by using the `echo` command. The `echo` command is used to display a line of text that is passed in as an argument. This is a built in bash command that is mostly used in shell scripts to output status text to the screen or a file. Place the following `echo` statements on the lines before each of the commands:

```bash
echo "Your current working directory is:"
pwd
echo "These are the contents of this directory:"
ls -l 
```

Now we are all set! You can save the file and exit `vim`. You should now be back at the command prompt. Check and see that the new script file you created is there:

```bash
$ ls -l
```

To run the shell script you can use the `bash` or `sh` command, followed by the name of your script:

```bash
$ sh listing.sh
```

> **Did it work like you expected?**
> 
> **Were the `echo` commands helpful in letting you know what came next?**

This is a very simple shell script, just to introduce you to the concept. Before we jump into more scripts which better demonstrate their utility, we will take a moment to cover some key concepts to help get you there.

***

**Exercise**

1. Open up the script `listing.sh` using vim. Add the command which prints to screen the contents of the file `Mov10_rnaseq_metadata.txt`.
2. Add an echo statement for the command, which tells the user "This is information about the files in our dataset:"
3. Run the new script. Provide the contents of the new script and the output after running it.

***

## Bash variables

A *variable* is a common concept shared by many programming languages. **Variables are essentially a symbolic/temporary name for, or a reference to, some information**. Variables are analogous to "buckets", where **information can be stored, maintained and modified** without too much hassle. Extending the bucket analogy: the bucket has a name associated with it, i.e. the name of the variable, and when referring to the information in the bucket, we use the name of the bucket, and do not directly refer to the actual data stored in it.

To create a variable in bash, you provide the name of the variable, followed by the equals sign, and finishing with tbe value we want to assign to the variable. Note that the variable name cannot contain spaces, nor can there be spaces on either side of the equals sign.

Let's start by creating a variable called `num` that has the number 25 stored inside it:

```bash
$ num=25
```

Once you press return, you will find yourself back at the command prompt. **How do we know that we actually created the bash variable?**

One way to see the variable created is by using the `echo` command. As we learned earlier, this command takes the argument provided and prints it to the terminal. If we provide `num` as an argument it will simply be interpreted as a character string. We want `echo` to display the contents of the variable and not its name.  To do this we need to **explicitly use a `$` in front of the variable name**:

```bash
$ echo $num
```

You should see the number 25 returned to you. Did you notice that when we created the variable, there was no need for a `$`? This is standard shell notation (syntax) for defining and using variables. When defining the variable (i.e. setting the value) you can just type it as is, but when **retrieving the value of a variable don't forget the `$`!** 

> **NOTE:** Variables are not physical entities like files. Whe you create files you can use `ls` to list contents and see if the file exists. When creating variables, to list all variables in your environment you can use the command `declare`. You will notice that while you only have created one variable so far, the output of `declare` will be more than just one variable. These other variables are called environment variables and will be [discussed in more detail later in the workshop](07_permissions_and_environment_variables.md).
> 
> If you use `declare`, try piping it to the `grep` command followed by the name of the variable so you trim that list to only display the variable you are interested in:
> 
> `declare | grep num`
> 

### Using variables as input to commands

So far, it is hard to see the utility of a variable and why we need it. One important aspect of the variable is that the value stored inside it can be used as input to commands. To demonstrate this we will create a new variable called `file`. We will store a character string as the value of the variable, specifically a filename:

```bash
$ file=Mov10_oe_1.subset.fq
```

Once you press return, you should be back at the command prompt. Let's check what's stored inside `file` using the `echo` command:

```bash
$ echo $file
```

Now let's use this variable `file` as input to one of the commands we previously learned:

```bash
$ wc -l $file
```

**What do you see in the terminal? What you were expecting `wc -l` to return to you?**

The `wc -l` command is used to count and report the number of lines in a file. Here, we provided a file but we did not get a number reported. Instead we got the error `wc: Mov10_oe_1.subset.fq: No such file or directory`. This is because the file that we listed does not exist in our current working directory. To get around this we can do one of two things.

1. Provide a path to the file:

```bash
$ wc -l ~/unix_lesson/raw_fastq/$file
```
OR 

2. Change directories to where the file lives: 

```bash
$ cd ~/unix_lesson/raw_fastq
$ wc -l $file
```

Either one of these options should have worked and you should see the number of lines in the file reported to you!

> **NOTE:** The variables we create in a session are system-wide, and independent of where you are in the filesystem. This is why we can reference it from any directory. However, it is only available for your current session. If you exit the cluster and login again at a later time, the variables you have created will no longer exist.

***

**Exercise**

1. Use the `$file` variable as input to the `head` and `tail` commands, and modify the arguments to display only four lines. Provide the lines of code used and report the header lines (`@HWI`) you retrieve from each command. 
2. Create a new variable called `meta` and assign it the value `Mov10_rnaseq_metadata.txt`. For the following questions, use the `$meta` variable but do not change directories. Provide the code you would run to:
	1. Display the contents of the file using `cat`.
	1. Retrieve only the lines which contain normal samples. (*Hint: use `grep`*)
***


### Assigning the output from a command to a variable

When creating shell scripts, variables are used to store information that can be used later in the script (once or many times over). The value stored can be hard-coded in as we have done above, assigning the variable a numeric or character value. Alternatively, the value stored can be the output of another command. We will demonstrate this using a new command called `basename`.

The **`basename` command** is used for extracting the base name of a file, which is accomplished using **string splitting to strip the directory and any suffix from filenames**. Let's try an example, by first moving back to your home directory:

```bash
$ cd
```

Then we will run the `basename` command on one of the FASTQ files. Be sure to specify the path to the file:

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq
```

**What is returned to you?**

The filename was split into the path `unix_lesson/raw_fastq/` and the filename `Mov10_oe_1.subset.fq`. The command **returns only the filename**. 

Now, suppose we wanted to also **trim off the file extension** (i.e. remove `.fq` leaving only the file *base name*). We can do this by adding a parameter to the command to specify what string of characters we want trimmed.

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq
```

You should now see that only `Mov10_oe_1.subset` is returned. 


***

**Exercise**

1. How would you modify the above `basename` command above to only return `Mov10_oe_1`?
2. Use `basename` with the file `Irrel_kd_1.subset.fq` as input. Return only `Irrel_kd_1` to the terminal.

***


The `basename` command returns a character string and this is totally something we can store inside a variable! To do this we need to use a special syntax because when we run the command we have spaces. If you remember earlier, one of the rules of creating variables is that there cannot be any spaces. The special syntax involves a key that is probably not used much on your keyboard, it is the backtick <kbd>`</kbd>. On most keyboards this character is located just underneath the <kbd>esc</kbd> key. If you have trouble finding it you can also justy copy and paste it from the materials.

The command that we are running is wrapped in backticks (one at the beginning and one at the end), and then we assign it to the variable as we would any other value. Let's try this with the `Mov10_oe_1.subset.fq` example from above:

```bash
$ base=`basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq`
```

Once you press return you should be back at the command prompt. Check to see what got stored in the `base` variable:

```bash
$ echo $base
```

## Advanced shell script

Bring all concepts in this lesson together!


In later lessons, we will be learning how to write more complex ones scripts to illustrate the power of scripts and how they can make our lives (when coding) much easier. Any type of data you will want to analyze will inevitably involve not just one step, but many steps and perhaps many different tools/software programs. Compiling these into a shell script is the first step in creating your analysis workflow!







---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


