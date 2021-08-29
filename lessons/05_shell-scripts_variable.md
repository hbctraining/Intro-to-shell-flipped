---
title: "Shell scripts and other related concepts"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu"
date: "Thursday, May 5, 2016"
---

Approximate time: 60 minutes

## Learning Objectives

* Capture previous commands into a shell script
* Describe variables in Shell and the different use cases
* Illustrate the use of variables in a shell script


## Shell scripts

By this point in the workshop you have been introduced to a number of commands to explore your data. To demonstrate the function of each command we have run them one at a time at the command prompt. The command prompt is useful for testing out commands and also performing simple tasks like exploring and organizing the file system. When we are running analyses which require a series of tasks to be run, there is a more efficent way to do this using shell scripts. 

Shell scripts are **text files that contain commands we want to run**. In this lesson we will introduce you to shell scripts by providing a simple example. We will also introduce bash variables and their use case with a more advanced shell script example.

> ### Review of Vim modes
>
> ***You will be using Vim quite a bit in this lesson, so before you get started, make sure you review the modes in vim.***
>
> | key              | action                 |
> | ---------------- | ---------------------- |
> | <button>i</button>     | insert mode - to write and edit text |
> | <button>esc</button>     | command mode - to issue commands / shortcuts  |

### A simple script

We are finally ready to see what makes the shell such a powerful programming environment. To create our first script, we are going to take some of the commands we have run previously and save them into a file so that we can **re-run all those operations** again later, by typing just **one single command**. For historical reasons, a bunch of commands saved in a file is referred to as shell script, but make no mistake, this is actually a small program! 

Interestingly, when working with Shell or on the command line you can give files any (or no) extension (.txt, .tsv, .csv, etc.). Similarly, for a shell script you don't need a specific extension. However, it is best practice to give shell scripts the extension `.sh`. This is helpful to your future self and your collaborators to identify that a given file is a shell script.

Move over to the `other` directory and create a new file using `vim`. We will call our script `listing.sh`:

```bash
$ cd ~/unix_lesson/other
$ vim listing.sh
```

This shell script will do two things:

1. Tell us our current working directory
2. List the contents of the directory 

We already know the commands for doing both of these things, so let's go into the insert mode in vim, and add the 2 commands into our script:

```bash
pwd
ls -l 
```

Now, we could save and quit and this shell script would run perfectly fine. But, we will add some verbosity to our script by using the `echo` command (verbosity is a good thing here!). The `echo` command is used to display a line of text that is passed in as an argument. This is a bash command that is mostly used in shell scripts to output status to the screen or to a file. 

Place the following `echo` statements on the lines before each of the commands:

```bash
echo "Your current working directory is:"
pwd

echo "These are the contents of this directory:"
ls -l 
```

Now we are all set! You can move to the command mode in vim now and save the file and exit `vim`. This should bring you back to the command prompt. Check and see that the new script file you created is in this directory:

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
3. Run the new script. Report the contents of the new script and the output you got after running it.

	<details>
		<summary><b><i>Answers</i></b></summary>
		<p><i>Question 1</i><br>
		Add this command to <code>listing.sh</code> using vim:<br>
		<code>cat Mov10_rnaseq_metadata.txt</code></p>
		<p><i>Question 2</i><br>
		Add this command to <code>listing.sh</code> using vim:<br>
		<code>echo "This is information about the files in our dataset:"</code></p>
		<p><i>Question 3</i><br>
		<code>sh listing.sh</code></p>
		<p><pre> Your current working directory is:
	/home/mm573/unix_lesson/other
	These are the contents of this directory:
	total 240
	-rw-rw-r-- 1 mm573 mm573  346 Sep 30 12:47 directory_info.sh
	-rw-rw-r-- 1 mm573 mm573  193 Oct  5 14:53 listing.sh
	-rw-rw-r-- 1 mm573 mm573   93 Sep 30 10:40 Mov10_rnaseq_metadata.txt
	-rw-r--r-- 1 mm573 mm573 1057 Sep 30 10:40 sequences.fa
	-rw-rw-r-- 1 mm573 mm573   48 Oct  5 14:49 spider.txt
	This is information about the files in our dataset:
	sample	celltype
	OE.1	Mov10_oe
	OE.2	Mov10_oe
	OE.3	Mov10_oe
	IR.1	normal
	IR.2	normal
	IR.3	normal</pre>
	</details>
***

## Bash variables

A *variable* is a common concept shared by many programming languages. **Variables are essentially a symbolic/temporary name for, or a reference to, some information**. Variables are analogous to "buckets", where **information can be stored, maintained and modified** without too much hassle. Extending the bucket analogy: the bucket has a name associated with it, i.e. the name of the variable, and when referring to the information in the bucket, we use the name of the bucket, and do not directly refer to the actual data stored in it.

To create a variable in bash, you provide the name of the variable, followed by the equals sign, and finishing with the value we want to assign to the variable. Note that the variable name cannot contain spaces, nor can there be spaces on either side of the equals sign.

Let's start by creating a variable called `num` that has the number 25 stored inside it:

```bash
$ num=25
```

Once you press return, you will find yourself back at the command prompt. **How do we know that we actually created the bash variable?**

One way to see the variable created is by using the `echo` command. As we learned earlier, this command takes the argument provided and prints it to the terminal. If we provide `num` as an argument it will be simply interpreted as a character string "num". We want `echo` to display the contents of the variable and not its name. To do this we need to **explicitly use a `$` in front of the variable name**:

```bash
$ echo $num
```

You should see the number 25 returned to you. Did you notice that when we created the variable, there was no need for a `$`? This is standard shell notation (syntax) for defining and using variables. When defining the variable (i.e. setting the value) you can just type it as is, but when **retrieving the value of a variable don't forget the `$`!** 

> **NOTE:** Variables are not physical entities like files. When you create files you can use `ls` to list contents and see if the file exists. When creating variables, to list all variables in your environment you can use the command `declare`. You will notice that while you only have created one variable so far, the output of `declare` will be more than just one variable. These other variables are called environment variables and will be [discussed in more detail later in the workshop](07_permissions_and_environment_variables.md).
> 
> If you use `declare`, try piping it to the `grep` command followed by the name of the variable so you trim that list to only display the variable you are interested in:
> 
> `declare | grep num`
> 

### Using variables as input to commands

So far, it is hard to see the utility of a variable and why we need it. One important aspect of the variable is that the value stored inside it can be used as input to commands. To demonstrate this we will create a new variable called `file`. We will store a character string as the value of the variable, specifically the name of one of the files in the `raw_fastq` directory:

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

* Provide a path to the file:

```bash
$ wc -l ~/unix_lesson/raw_fastq/$file
```
OR 

* Change directories to where the file lives: 

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
	2. Retrieve only the lines which contain normal samples. (*Hint: use `grep`*).  

	<br><details>
		<summary><b><i>Answers</i></b></summary>
		<p><i>Question 1</i><br>
		<code>head -n 4 $file</code><br>
		@HWI-ST330:304:H045HADXX:1:1101:1162:205<br></p>
		<p><code>head -n 4 $file</code><br>
		@HWI-ST330:304:H045HADXX:2:2212:15724:100530</p>
		<i>Question 2</i><br>
		<p><i>Part i</i><br>
		<code>cat ../other/$meta</code> (relative path) or <br> 
		<code>cat ~/unix_lesson/other/$meta</code> (full path)<br></p>
		<p style="margin-left: 40px"><i>Part ii</i><br>
		<code>grep normal ../other/$meta</code> (relative path) or<br>
       		<code>grep normal ~/unix_lesson/other/$meta</code> (full path)<br></p>
	</details>
***


### Assigning the output from a command to a variable

When creating shell scripts, variables are used to store information that can be used later in the script (once or many times over). The value stored can be hard-coded in as we have done above, assigning the variable a numeric or character value. Alternatively, the value stored can be the output of another command. We will demonstrate this using a new command called `basename`.

The **`basename` command** is used for extracting the base name from a file path, which is accomplished using **string splitting**. Let's try an example, by first moving back to your home directory:

```bash
$ cd
```

Then we will run the `basename` command on one of the FASTQ files. Be sure to specify the path to the file:

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq
```

**What is returned to you?**

The path was split into all text leading up to the last `/` (which is `~/unix_lesson/raw_fastq/`) and everything after the `/` which is the file name `Mov10_oe_1.subset.fq`. The command **returns only the file name**. 

Now, suppose we wanted to also **trim off the file extension** (i.e. remove `.fq` leaving only the file *base name*). We can do this by **adding a parameter** to the command to specify what string of characters we want trimmed.

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq
```

You should now see that only `Mov10_oe_1.subset` is returned. 


***

**Exercise**

1. How would you modify the above `basename` command above to only return `Mov10_oe_1`?
2. Use `basename` with the file `Irrel_kd_1.subset.fq` as input. Return only `Irrel_kd_1` to the terminal.

	<details>
		<summary><b><i>Answers</i></b></summary>
		<p><i>Question 1</i><br>
		<code>basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .subset.fq</code></p>
		<p><i>Question 2</i><br>
		<code>basename ~/unix_lesson/raw_fastq/Irrel_kd_1.subset.fq .subset.fq</code></p>

***


The `basename` command returns a character string and this is totally something we can store inside a variable! To do this we need to use a special syntax because when we run the command we have spaces. If you remember earlier, one of the rules of creating variables is that there cannot be any spaces. 

> **NOTE:** The special syntax involves a key that is probably not used much on your keyboard, it is **the backtick key** <kbd>`</kbd>. On most keyboards this character is located just underneath the <kbd>esc</kbd> key. If you have trouble finding it you can also just copy and paste it from the materials.

The command that we are running is wrapped in backticks (one at the beginning and one at the end), and then we assign it to the variable as we would any other value. Let's try this with the `Mov10_oe_1.subset.fq` example from above:

```bash
$ samplename=`basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq`
```

Once you press return you should be back at the command prompt. Check to see what got stored in the `samplename` variable:

```bash
$ echo $samplename
```

> #### The `basename` command
> It is hard to see the utility of this command by just running it at command-line, but it is very useful command when creating scripts for analysis. Within a script it is common to create an output file and the `basename` allows us to easily create a prefix to use for naming the output files. We will demonstrate this in more detail during our ["Loops and Automation" lesson](04_loops_and_scripts.md).


## Shell scripting with bash variables

Now it's time to put all of these concepts together to create a more advanced version of the script that we started with at the beginning of this lesson! This script will allow the user to get information on any given directory. These are the steps we will code into our shell script:

1. Assign the path of the directory to a variable
2. Create a variable that stores only the directory name (and no path information)
3. Move from the current location in the filesystem into the directory we selected in 1.
4. List the contents of the directory
5. List the total number of files in the directory


It seems like a lot, but you are equipped with all the necessary concepts and commands to do this quite easily!

Let's get started by moving into the `other` directory and creating a script called `directory_info.sh`:

```bash
$ cd ~/unix_lesson/other
$ vim directory_info.sh
```

In this script, we will be adding **comments by using the hashtag symbol `#`**. Almost all lines in your script that begin with `#` will not be interpreted as code by Shell. Comments are crucial for proper documentation of your scripts. This will allow your collaborators or your future self to know what each line of code is doing. 

We will begin with a first comment describing the **usage of this script**. This lets anyone who is using the script know what it does and what they need to provide (if anything). In our case we need the user to provide a path to the directory of interest. This will be assigned to a variable for use later in the script.

```bash
## USAGE: Provide the full path to the directory you want information on
dirPath=~/unix_lesson/raw_fastq
```

Next, we will create another variable to store the directory name. To get the directory name we can use `basename` command and extract it from the user provided path. *Note the use of the `$` to retrieve the value stored inside the variable!*

```bash
# Get only the directory name
dirName=`basename $dirPath`
```

The next few tasks we want to execute require simple commands for changing directories (`cd`), listing contents of a directory (`ls -l`). We can add these into our script making sure we are referencing the correct variable and also including meaningful `echo` statements for verbosity.

```bash
echo "Reporting on the directory" $dirName "..."

# Move into the directory
cd $dirPath

echo "These are the contents of" $dirName
ls -l 
```

The final task of reporting the total number of files will require us to pipe (`|`) together multiple commands:

```bash
echo "The total number of files contained in" $dirName
ls | wc -l

echo "Report complete!"
```

After adding in a final `echo` statement, we are all set with script! Make sure that your script looks similar to what we have listed below. If it does, save and exit Vim. 

```bash
## USAGE: Provide the full path to the directory you want information on
dirPath=~/unix_lesson/raw_fastq

# Get only the directory name
dirName=`basename $dirPath`

echo "Reporting on the directory" $dirName "..."

# Move into the directory
cd $dirPath

echo "These are the contents of" $dirName
ls -l 

echo "The total number of files contained in" $dirName
ls | wc -l

echo "Report complete!"
```

***

**Exercise**

1. Run the script `directory_info.sh`. Report what gets printed to the screen.
2. Open up the script `directory_info.sh` using vim. Change the approproiate line of code so that our directory of interest is `~/unix_lesson/genomics_data`. Save and exit Vim.
3. Run the script with the changes and report what gets printed to the screen.

	
	<details>
		<summary><b><i>Answers</i></b></summary>
		<p><i>Question 1</i><br>
		<code>sh directory_info.sh</code></p>
	<pre> Reporting on the directory raw_fastq ...
	These are the contents of this directory:
	total 384128
	-rw-rw-r-- 1 mm573 mm573 55169229 Sep 30 10:40 Irrel_kd_1.subset.fq
	-rw-rw-r-- 1 mm573 mm573 47460403 Sep 30 10:40 Irrel_kd_2.subset.fq
	-rw-rw-r-- 1 mm573 mm573 36268325 Sep 30 10:40 Irrel_kd_3.subset.fq
	-rw-rw-r-- 1 mm573 mm573 75706556 Sep 30 10:40 Mov10_oe_1.subset.fq
	-rw-rw-r-- 1 mm573 mm573 68676755 Sep 30 10:40 Mov10_oe_2.subset.fq
	-rw-rw-r-- 1 mm573 mm573 42742047 Sep 30 10:40 Mov10_oe_3.subset.fq
	The total number of files in this directory is:
	6</pre>
		<p><i>Question 2</i><br>
		Change:<br>
		<code>dirPath=~/unix_lesson/raw_fastq</code> to:<br>
		<code>dirPath=~/unix_lesson/genomics_data</code></p>
		<p><i>Question 3</i><br>
		<code>sh directory_info.sh</code></p>
	<pre> Reporting on the directory raw_fastq ...
	These are the contents of this directory:
	total 26240
	-rwxrwxr-- 1 mm573 mm573   130904 Sep 30 10:40 Encode-hesc-Nanog.bed
	-rw-rw-r-- 1 mm573 mm573 21893006 Sep 30 10:40 na12878_q20_annot.vcf
	The total number of files in this directory is:
	2</pre>
	</details>
			
			

***

In this lesson, we described shell scripts and introduced a few related concepts that are helpful when you are starting out. It is important to understand each of the indvidual concepts, but also to see how they all come together to add flexibility and efficency to your script. Later in the workshop we will further illustrate the power of scripts and how they can make our lives (when coding) much easier. Any type of data you will want to analyze will inevitably involve not just one step, but many steps and perhaps many different tools/software programs. Compiling these into a shell script is the first step in creating your analysis workflow!

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


