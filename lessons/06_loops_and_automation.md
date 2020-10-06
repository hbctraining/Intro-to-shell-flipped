---
title: "The Shell: Loops and automation"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu"
date: "Thursday, May 5, 2016"
---

Approximate time: 60 minutes

## Learning Objectives

* Introduce the concept of a `for loop` to iterate commands over multiple items
* Automating tasks by using loops inside shell scripts


## Loops

Typically, when you are running analyses on the cluster, you are running multiple commands which correspond to inidvidual steps in your workflow. We learned earlier that we can compile these commands into a single shell script to make this process more efficient. What if we could further increase our efficiency so that the same series of commands could be easily repeated for each indvidual sample in our dataset? We can do this with the use of loops in Shell!

Looping is a concept shared by several programming languages, and its implementation in bash is very similar to other languages. 

The structure or the syntax of (*for*) loops in bash is as follows:

```bash
for (variable_name) in (list)
	do
	(command1 $variable_name)
	(command2 $variable_name)
	...
	....
	done
```

The text that is **red, are parts of the loop structure that remain constant**. That is, for every loop your create you will need to have the words: `for`, `in`, `do` and `done`. *This syntax/structure is virtually set in stone.* The text that goes in between those words will change depending on what it is you want your loop to do.

### How do loops work?

Let's use the example below to go through step-by-step how a loop is actually working.

```bash
for x in Mov10_oe_1.subset.fq  Mov10_oe_2.subset.fq Mov10_oe_3.subset.fq
 do
   echo $x
   wc -l $x
 done
```

1. When we start the loop, we define a temporary ***variable_name*** and a **list** of things that we would like to iterate over. In our example, the temporary variable is called `x` and the list is the filenames for all of the Mov10 samples in our dataset. The variable is initialized by taking the value of the first item in the list. 

> **We don't explictly see this, but the variable has been defined as `x=Mov10_oe_1.subset.fq`.**

2. Next, all of the commands in the body of the loop (between the `do` and `done`) are executed. Usually, the commands placed here will be using the temporary variable as input. **Remember, if you are using the value stored in the variable you need to use $ to reference it!** In the example, we are running two commands:

* `echo $x`: print out the value stored in `x`
* `wc -l $x`: count/report the number of lines in `x`

3. Once those two commands are complete, the temporary variable is assigned a new value. It now takes the value of the second item in the list.

> **The variable is reassigned a value `x=Mov10_oe_2.subset.fq`.**

4. Once again, all of the commands in between the `do` and `done` are executed. This time they are using the new value stored in `x` as input.

5. The temporary variable then takes on the value of the third item in the list.

> **The variable is reassigned a value `x=Mov10_oe_3.subset.fq`.**

6. Once again, all of the commands in between the `do` and `done` are executed using the new value stored in `x`. 

7. Now that we have gone through every item in the list, the loop is `done` and it exits. 

Essentially, **the number of items in the list == number of times the code will loop through**. So in our case, we had three files listed and so the series of commands in the body of the loop were repeated three times. If we had provided all six files, the series of commands would be repeated six times.


### Best practices

In this case the list of files is specified using the asterisk wildcard: `*.fq`, i.e. all files that end in `.fq`. 

> **What else could we have used in place of the `ls *.fq`?**

Then, we execute 2 commands between the `do` and `done`. With a loop, we execute these commands for each file at a time. Once the commands are executed for one file, the loop then executes the same commands on the next file in the list. 



It doesn't matter what variable name we use, but it is advisable to make it something more intuitive. In the long run, it's best to use a name that will help point out a variable's functionality, so your future self will understand what you are thinking now.



## Automating with Scripts
	
Now we're ready to incorporate these new concepts into a complex shell script and put this processing power to work. Imagine, if you will, a script that will run a series of commands that would do the following for us each time we get a new data set:

- Use for loop to iterate over each FASTQ file
- Generate a prefix to use for naming our output files
- Dump out bad reads into a new file
- Get the count of the number of bad reads and generate a summary file

You might not realize it, but this is something that you now know how to do. Let's get started...

Rather than doing all of this in the terminal we are going to create a script file with all relevant commands. Move back in to `unix_lesson` and use `vim` to create our new script file:

```bash
$ cd ~/unix_lesson

$ vim generate_bad_reads_summary.sh
```

We always want to start our scripts with a shebang line: 

```bash
#!/bin/bash
```

This line is the absolute path to the Bash interpreter. The shebang line ensures that the bash shell interprets the script even if it is executed using a different shell.

After the shebang line, we enter the commands we want to execute. First we want to move into our `raw_fastq` directory:

```bash
# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq
```

And now we loop over all the FASTQs:

```bash
# enter directory with raw FASTQs
for filename in *.fq
```

For each file that we process we can use `basename` to create a variable that will uniquely identify our output file based on where it originated from:

```bash
do
  # create a prefix for all output files
  base=`basename $filename .subset.fq`
```

and then we execute the commands for each loop:

```bash
  # tell us what file we're working on
  echo $filename
  
  # grab all the bad read records into new file
  grep -B1 -A2 NNNNNNNNNN $filename > ${base}-badreads.fastq
``` 
  
We'll also count the number of these reads and put that in a new file, using the count flag of `grep`:

```bash
  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> badreads.count.summary
done
```

If you've noticed, we used a new `grep` flag `-H` above; this flag will report the filename along with the match string. This is useful for when we generate the summary file and we know what number associates with which file.

Save and exit `vim`, and voila! You now have a script you can use to assess the quality of all your new datasets. Your finished script, complete with comments, should look like the following:

```bash
#!/bin/bash 

# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq

# count bad reads for each FASTQ file in our directory
for filename in *.fq 
do 

  # create a prefix for all output files
  base=`basename $filename .subset.fq`

  # tell us what file we're working on	
  echo $filename

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $filename > ${base}-badreads.fastq

  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> badreads.count.summary
done

```

To run this script, we simply enter the following command:

```bash
$ sh generate_bad_reads_summary.sh
```

How do we know if the script worked? Take a look inside the `raw_fastq` directory, we should see that for every one of the original FASTQ files we have one bad read file. We should also have a summary log file documenting the total number of bad reads from each file.

```bash
$ ls -l ~/unix_lesson/raw_fastq 
```

To keep our data organized, let's move all of the bad read files out of the `raw_fastq` directory into a new directory called `other`, and the script to a new directory called `scripts`.

```bash
$ mv raw_fastq/*bad* other/

$ mkdir scripts
$ mv *.sh scripts/
```

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


