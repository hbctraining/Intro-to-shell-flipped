---
title: "The Shell: Loops and automation"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu"
date: "Thursday, May 5, 2016"
---

Approximate time: 60 minutes

## Learning Objectives

* Learn how to use variables to operate on multiple files




## Loops

Another powerful concept in the Unix shell and useful when writing scripts is the concept of "Loops". We have just shown you that you can run a single command on multiple files by creating a variable whose values are the filenames that you wish to work on. But what if you want to **run a sequence of multiple commands, on multiple files**? This is where loop come in handy!

Looping is a concept shared by several programming languages, and its implementation in bash is very similar to other languages. 

The structure or the syntax of (*for*) loops in bash is as follows:

```bash
for (variable_name) in (list)
do
(command1 $variable_name)
.
.
done
```

where the ***variable_name*** defines (or initializes) a variable that takes the value of every member of the specified ***list*** one at a time. At each iteration, the loop retrieves the value stored in the variable (which is a member of the input list) and runs through the commands indicated between the `do` and `done` one at a time. *This syntax/structure is virtually set in stone.* 


#### What does this loop do? 

```bash
for x in *.fq
 do
   echo $x
   wc -l $x
 done
```

Most simply, it writes to the terminal (`echo`) the name of the file and the number of lines (`wc -l`) for each files that end in `.fq` in the current directory. The output is almost identical to what we had before.

In this case the list of files is specified using the asterisk wildcard: `*.fq`, i.e. all files that end in `.fq`. 

> **What else could we have used in place of the `ls *.fq`?**

Then, we execute 2 commands between the `do` and `done`. With a loop, we execute these commands for each file at a time. Once the commands are executed for one file, the loop then executes the same commands on the next file in the list. 

Essentially, **the number of items in the list (variable name) == number of times the code will loop through**, in our case that is 2 times since we have 2 files in `~/unix_lesson/raw_fastq` that end in `.fq`, and these filenames are stored in the `filename` variable.

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


