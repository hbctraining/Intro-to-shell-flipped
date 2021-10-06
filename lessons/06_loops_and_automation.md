---
title: "The Shell: Loops and automation"
author: "Meeta Mistry, Bob Freeman, Mary Piper, Radhika Khetani, Jihe Liu, Will Gammerdinger"
date: "October 2021"
---

Approximate time: 60 minutes

## Learning Objectives

* Describe the concept of 'looping' to iterate commands over multiple items
* Automate tasks by using loops inside shell scripts


## Loops

Typically, when you are running analyses on the cluster, you are running multiple commands which correspond to individual steps in your workflow. We learned earlier that we can compile these commands into a single shell script to make this process more efficient. What if we could further increase our efficiency so that the same series of commands could be easily repeated for each sample in our dataset? We can do this with the use of loops in Shell!

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
$ cd ~/unix_lesson/raw_fastq/

$ for x in Mov10_oe_1.subset.fq Mov10_oe_2.subset.fq Mov10_oe_3.subset.fq
 do
   echo $x
   wc -l $x
 done
```

|    Loop component      |      Value          |
| ---------------- | ---------------------- |
| ***variable_name*** | `x` |
| **list** | `Mov10_oe` FASTQ files |
| ***body (commands to be executed)*** | `echo` and `wc -l` |


> #### Running loops at the command prompt
> In our materials, the for loop is written out using multiple lines rather than the single line commands we have been running so far. When running this at the command prompt begin by typing out the `for` statement, then press the return key. You will notice that you are not back at your command prompt. Rather than a `$`, you should see a `>`. The shell has acknowledged that you have started a for loop and is waiting for you to complete it. Continue to type code line by line. Once you type in `done` and press return the shell will know you are done and will run the loop. 

> > **NOTE:** Use your <button> up </button> arrow at the command prompt to traverse through your history and see the for loop that you just ran. Even though we typed it out on multiple lines, it shows up on a single line. You can also create for loops using this syntax although we don't recommend it as it can be hard to read.


1. When we start the loop, the temporary variable is initialized by taking the value of the first item in the list. 

	> **We don't explicitly see this, but the variable has been defined as `x=Mov10_oe_1.subset.fq`.**

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


### Creating loops using best practices

#### Meaningful variable names
It doesn't matter what variable name we use, but it is advisable to make it something more intuitive. In the long run, it's best to use a name that will help point out a variable's functionality, so your future self will understand what you are thinking now.

#### Using the wildcard to define the list 
In the example above, we typed out each item in the list leaving a space in between each item. This is usually fine for one or two items, but with larger lists this can become tedious and error-prone. If the list you are iterating over share some similarities in the naming we recommend using the wildcard shortcut to specify the list. For example, instead of typing out each Mov10 filename we could list them using `Mov*.fq`.

**But, if I am listing the files don't I need to use the `ls` command in the first line of my loop?**

No, you don't. Even though it seems that way intuitively, when using a `for` loop the `ls` is implied. 

Let's rewrite the for loop above using a more meaningful variable name and using the wildcard:

```bash
$ for file in Mov10*.fq
 do
   echo $file
   wc -l $file
 done
```

> **Don't forget to change the name of the variable being referenced inside of the loop!**

***

**Exercise**

1. Change the for loop example above so that it runs on all six FASTQ files.
2. Change the for loop example above so that it prints out the first line of all six files.

***


## Automating with Scripts

We've already had some fun with shell scripts, but now it's time to take our shell scripting to another level by incorporating loops! Imagine, if you will, a script that would do the following for us each time we get a new data set:

- Use for loop to iterate over each FASTQ file
- Generate a prefix to use for naming our output files
- Dump out bad reads into a new file
- Get the count of the number of bad reads and report it to a running log file

It might seem daunting, but everything outlined above is something that you know how to do. Let's get started...

We will first create a directory for any ouput files that we generate.

```bash
$ mkdir ~/unix_lesson/badreads
```

Now, move back in to the `badreads` directory and use `vim` to create our new script file:

```bash
$ cd ~/unix_lesson/badreads

$ vim generate_bad_reads_summary.sh
```

At the beginning of our script we are going to add what is called a **shebang line**. This line is the absolute path to the Bash interpreter. The shebang line ensures that the bash shell interprets the script even if it is executed using a different shell.

> #### Why do I need a shebang line? My scripts ran perfectly well before without it.
> Having a shebang line is best practice. While your script will run fine without it in environments where bash is the default shell, it won't if the user of this script is using a different shell. To avoid any issues, we explicitly state that this script needs to executed using the bash shell.

```bash
#!/bin/bash
```

After the shebang line, we enter the commands we want to execute. First we want to move into our `raw_fastq` directory. 

> *Note the use of comments using the `#` symbol. Always comment liberally when creating your scripts!*

```bash
# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq
```

And now we loop over all the FASTQ files:

```bash
# loop over each FASTQ file
for filename in *.fq
```

Type out `do` followed by all the things we need to do.

For each file that we process we can use `basename` to create a prefix from the original filename. We will store this prefix in a variable called `samplename` and use it to to uniquely label our output files.

> *Note the use of backticks to assign the output of the `basename` command as value to the variable! If you cannot find the backtick key on your keyboard, just copy and paste from the lesson.*

```bash
do
  # create a prefix for all output files
  samplename=`basename $filename .subset.fq`
```

Now we execute the command required to dump the bad reads to file, but first start with an `echo` statement to keep the user informed. We will use `grep` to find all the bad reads (in our case, bad reads are defined as those with 10 consecutive N's), and then extract the four lines associated with each sequence read and write them to a file. Our output file is named using the `samplename` variable we created earlier in the loop. We will also add a path to redirect output to the `badreads` directory.

```bash
  # tell us what file we're working on
  echo $filename
  
  # grab all the bad read records into new file
  grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/${samplename}_badreads.fq
``` 

> #### Why are we using curly brackets with the variable name?
> When we append a variable to some other free text, we need shell to know where our variable name ends. By encapsulating the variable name in curly brackets we are letting shell know that everything inside it is the variable name. This way when we reference it, shell knows to print the variable `$base` and not to look for a variable called `$base-badreads.fq`.

  
We'll also count the number of identified bad reads using the count flag of `grep`, `-c`, which will return the number of matches rather than the actual matching lines. Here, we also use a new `grep` flag `-H`; this will report the filename along with the count value. This is useful because we are writing this information to a running log summary file, so rather than just reporting a count value we also know which file it is associated with.

```bash
  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/badreads.count.summary
done
```

Close the loop with `done`. Save and exit `vim`, and voila! You now have a script you can use to assess the quality of all your new datasets. Your finished script, complete with comments, should look like the following:

```bash
#!/bin/bash 

# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq

# loop over each FASTQ file
for filename in *.fq 
do 

  # create a prefix for all output files
  samplename=`basename $filename .subset.fq`

  # tell us what file we're working on	
  echo $filename

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/${samplename}_badreads.fq

  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/badreads.count.summary
done

```

To run this script, we simply enter the following command:

```bash
$ sh generate_bad_reads_summary.sh
```

How do we know if the script worked? Take a look inside the `badreads` directory, we should see that for every one of the original FASTQ files we have one bad read file. We should also have a summary log file documenting the total number of bad reads from each file.

```bash
$ ls -l ~/unix_lesson/badreads
```


---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*


