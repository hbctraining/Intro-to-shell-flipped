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

## Saving time with tab completion, wildcards and other shortcuts 

### Tab completion

Typing out full directory names can be time-consuming and error-prone. One way to avoid that is to use `tab` completion. The `tab` key is located on the left side of your keyboard, right above the `caps lock` key. When you start typing out the first few characters of a directory name, then hit the `tab` key, the shell will try to fill in the rest of the directory name. For example, first type `cd` to get back to your home directly, then type `cd uni`, followed by pressing the `tab` key:

```bash
$ cd
$ cd uni<tab>
```

The shell will fill in the rest of the directory name for `unix_lesson`. Now further go to `unix_lesson/raw_fastq`, then type `ls Mov10_oe_`, followed by `tab` key:

```bash
$ cd raw_fastq/
$ ls Mov10_oe_<tab><tab>
```

When you hit `tab` just for once, nothing happens. The reason is that there are multiple directories in the current directory that start with `Mov10_oe_`. As a result, shell does not know which one to fill in. When you hit `tab` again, the shell will then list all the possible choices.

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

So how does this actually work? The shell (bash) considers an asterisk "*" to be a wildcard character that can match one or more occurrences of any character, including no character. 

> An asterisk/star is only one of the many wildcards in UNIX, but this is the most powerful one and we will be using this one the most for our exercises.

****

**Exercise**

Do each of the following using a single `ls` command without
navigating to a different directory.

1.  List all of the files in `/bin` that start with the letter 'c'
2.  List all of the files in `/bin` that contain the letter 'a'
3.  List all of the files in `/bin` that end with the letter 'o'

BONUS: List all of the files in `/bin` that contain the letter 'a' or 'c'.

****


### Shortcuts

There are some very useful shortcuts that you should also know about. Dealing with the
home directory is very common. In shell, the tilde character,
"~", is a shortcut for your home directory. Let's first navigate to the `raw_fastq`
directory:

```bash
$ cd
$ cd unix_lesson/raw_fastq
```

Then enter the command:

```bash
$ ls ~
```

This prints the contents of your home directory, without you having to type the full path. This is because the tilde "~" is equivalent to "/home/username".

Another shortcut is "..":

```bash
$ ls ..
```

The shortcut `..` always refers to the directory above your current directory. So, it prints the contents of the `unix_lesson`. You can also chain these together:

```bash
$ ls ../..
```

This prints the contents of `/home/username`, which is two levels above your current directory (your home directory). 

Finally, the special directory `.` always refers to your current directory. So, `ls` and `ls .` will do the same thing - they print the contents of the current directory. This may seem like a useless shortcut, but recall that we used it earlier when we copied over the data to our home directory.

To summarize, the commands `ls ~`, `ls ~/.`, and `ls /home/username` all do exactly the same thing. These shortcuts can be convenient when you navigate through directories!

### Command History

You can easily access previous commands by hitting the up arrow on your keyboard. You can step backwards through your command history. On the other hand, the down arrow takes your forwards in the command history.

'Ctrl-r' will do a reverse-search through your command history.  This
is very useful.

You can also review your recent commands with the `history` command.  Just enter:

```bash
$ history
```

to see a numbered list of recent commands, including this just issued
`history` command. Only a certain number of commands are stored and displayed with `history`, there is a way to modify this to store a different number.

> **NOTE:** So far we have only run very short commands that have few or no arguments. It would look faster to just retype it than to check the history. However, as you start to run analyses on the commadn-line, you will find your commands to be more complexed, and the history will be very useful then!

**Other handy command-related shortcuts**

- <button>Ctrl + C</button> will cancel the command you are writing, and give you a fresh prompt.
- <button>Ctrl + A</button> will bring you to the start of the command you are writing.
- <button>Ctrl + E</button> will bring you to the end of the command.

**Exercise**

1.  Checking the output of the `history` command, how many commands have you typed in so far?
2.  Use the up arrow key to check the command you used before `history` command. What is it? Does it make sense?

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
