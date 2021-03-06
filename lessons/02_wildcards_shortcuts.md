---
title: "The Shell"
author: "Sheldon  McKay, Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu"
date: "August 7, 2017"
---

## Learning Objectives
- Become efficient when using the command-line interface (save time and prevent errors!)
  - Describe the use of tab completion when writing paths
  - Describe the use of the asterisk `*` wildcard when selecting multiple items
  - List a few shortcuts 

## Are you logged into O2 and on a compute node?

You need to be logged into O2, and be on a compute node to run through this lesson. If you are, please proceed to the next section!

If you are not logged into O2 or are not on a compute node, please follow the steps below as appropriate:

1. Log in using `ssh rc_trainingXX@o2.hms.harvard.edu` and enter your password (HMSXXcluster) (replace the "XX" with the number you were assigned in class) 
2. Once you are on the login node, use `srun --pty -p interactive -t 0-2:30 --mem 1G /bin/bash` to get on a compute node.
3. Proceed to the next section once your command prompt has the word `compute` in it.

## Saving time with tab completion, wildcards and other shortcuts 

### Tab completion

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

Tab completion can also fill in the names of commands. For example, enter `e<tab><tab>`. You will see the name of every command that starts with an `e`. One of those is `echo`. If you enter `ech<tab>`, you will see that tab completion works. 

> **Tab completion is your friend!** It helps prevent spelling mistakes, and speeds up the process of typing in the full command.


### Wild cards

Navigate to the `~/unix_lesson/raw_fastq` directory. This directory contains FASTQ files from a next-generation sequencing dataset. 

The "*" character is a shortcut for "everything". Thus, if you enter `ls *`, you will see all of the contents of a given directory. Now try this command:

```bash
$ ls *fq
```

This lists every file that ends with a `fq`. Try this command:

```bash
$ ls /usr/bin/*.sh
```

This lists every file in `/usr/bin` directory that ends in the characters `.sh`. "*" can be placed anywhere in your pattern. For example:

```bash
$ ls Mov10*fq
```

This lists only the files that begin with 'Mov10' and end with `fq`.

So how does this actually work? The Shell (bash) considers an asterisk "*" to be a wildcard character that can match one or more occurrences of any character, including no character. 

> **Tip** - An asterisk/star is only one of the many wildcards in Unix, but this is the most powerful one and we will be using this one the most for our exercises.

****

**Exercise**

Do each of the following using a single `ls` command without
navigating to a different directory.

1.  List all of the files in `/bin` that start with the letter 'c'
2.  List all of the files in `/bin` that contain the letter 'a'
3.  List all of the files in `/bin` that end with the letter 'o'

BONUS: Using one command to list all of the files in `/bin` that contain either 'a' or 'c'.

****


### Shortcuts

There are some very useful shortcuts that you should also know about. 

#### Home directory or "~"

Dealing with the home directory is very common. In shell, the tilde character, "~", is a shortcut for your home directory. Let's first navigate to the `raw_fastq` directory (try to use tab completion here!):

```bash
$ cd
$ cd unix_lesson/raw_fastq
```

Then enter the command:

```bash
$ ls ~
```

This prints the contents of your home directory, without you having to type the full path. This is because the tilde "~" is equivalent to "/home/username", as we had mentioned in the previous lesson.

#### Parent directory or ".."

Another shortcut you encountered in the previous lesson is "..":

```bash
$ ls ..
```

The shortcut `..` always refers to the parent directory of whatever directory you are in currently. So, `ls ..` will print the contents of `unix_lesson`. You can also chain these `..` together, separated by `/`:

```bash
$ ls ../..
```

This prints the contents of `/home/username`, which is two levels above your current directory (your home directory). 

#### Current directory or "."

Finally, the special directory `.` always refers to your current directory. So, `ls` and `ls .` will do the same thing - they print the contents of the current directory. This may seem like a useless shortcut, but recall that we used it earlier when we copied over the data to our home directory.

To summarize, the commands `ls ~`, `ls ~/.`, and `ls /home/username` all do exactly the same thing. These shortcuts can be convenient when you navigate through directories!

### Command History

You can easily access previous commands by hitting the <button>up</button> arrow key on your keyboard, this way you can step backwards through your command history. On the other hand, the <button>down</button> arrow key takes you forward in the command history.

***Try it out! While on the command prompt hit the <button>up</button> arrow a few times, and then hit the <button>down</button> arrow a few times until you are back to where you started.***

You can also review your recent commands with the `history` command. Just enter:

```bash
$ history
```

You should see a numbered list of commands, including the `history` command you just ran! 

Only a certain number of commands can be stored and displayed with the `history` command by default but you can increase or decrease it to a different number. It is outside the scope of this workshop, but feel free to look it up after class.

> **NOTE:** So far we have only run very short commands that have very few or no arguments. It would be faster to just retype it than to check the history. However, as you start to run analyses on the command-line you will find that the commands are longer and more complex, and the `history` command will be very useful then!

**Other handy command-related shortcuts**

- <button>Ctrl</button> + <button>C</button> will cancel the command you are writing, and give you a fresh prompt.
- <button>Ctrl</button> + <button>A</button> will bring you to the start of the command you are writing.
- <button>Ctrl</button> + <button>E</button> will bring you to the end of the command.

****

**Exercise**

1.  Checking the output of the `history` command, how many commands have you typed in so far?
2.  Use the <button>up</button> arrow key to check the command you typed before the `history` command. What is it? Does it make sense?
3.  Type several random characters on the command prompt. Can you bring the cursor to the start with <button>Ctrl</button> + <button>A</button>? Next, can you bring the cursor to the end with <button>Ctrl</button> + <button>E</button>? Finally, what happens when you use <button>Ctrl</button> + <button>C</button>?

****

## Commands, options, and keystrokes covered

```
~           # home dir
.           # current dir
..          # parent dir
*           # wildcard
ctrl + c    # cancel current command
ctrl + a    # start of line
ctrl + e    # end of line
history
```

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*
