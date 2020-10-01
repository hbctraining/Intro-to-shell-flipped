---
title: "The Shell"
author: "Sheldon  McKay, Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu"
date: "August 7, 2017"
---

## Learning Objectives
- How do you access the shell?
- How do you use it?
  - Getting around the Unix file system
  - looking at files
  - manipulating files
  - automating tasks
- What is it good for?


### Saving time with tab completion, wildcards and other shortcuts 

#### Tab completion

Typing out full directory names can be time-consuming and error-prone. One way to avoid that is to use `tab` completion. The `tab` key is located on the left side of your keyboard, right above the `caps lock` key. When you start typing out the first few characters of a directory name, then hit the `tab` key, the shell will try to fill in the rest of the directory name. For example, first type `cd` to get back to your home directly, then type `cd uni`, followed by `tab` key:

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

#### Wild cards

Navigate to the `~/unix_lesson/raw_fastq` directory. This directory contains FASTQ files from a next-generation sequencing dataset. 

The '*' character is a shortcut for "everything". Thus, if you enter `ls *`, you will see all of the contents of a given directory. Now try this command:

```bash
$ ls *fq
```

This lists every file that ends with a `fq`. This command:

```bash
$ ls /usr/bin/*.sh
```

Lists every file in `/usr/bin` that ends in the characters `.sh`.

```bash
$ ls Mov10*fq
```

lists only the files that begin with 'Mov10' and end with 'fq'

So how does this actually work? The shell (bash) considers an asterisk "*" to be a wildcard character that can be used to substitute for any other single character or a string of characters. 

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


#### Shortcuts

There are some shortcuts which you should know about. Dealing with the
home directory is very common. So, in the shell the tilde character,
"~", is a shortcut for your home directory. Navigate to the `raw_fastq`
directory:

```bash
$ cd
```

```bash
$ cd unix_lesson/raw_fastq
```

Then enter the command:

```bash
$ ls ~
```

This prints the contents of your home directory, without you having to type the full path because the tilde "~" is equivalent to "/home/username".

Another shortcut is the "..":

```bash
$ ls ..
```

The shortcut `..` always refers to the directory above your current directory. So, it prints the contents of the `unix_lesson`. You can chain these together, so:

```bash
$ ls ../..
```

prints the contents of `/home/username` which is your home directory. 

Finally, the special directory `.` always refers to your current directory. So, `ls`, `ls .`, and `ls ././././.` all do the same thing, they print the contents of the current directory. This may seem like a useless shortcut right now, but we used it earlier when we copied over the data to our home directory.


To summarize, while you are in your home directory, the commands `ls ~`, `ls ~/.`, and `ls /home/username` all do exactly the same thing. These shortcuts are not necessary, but they are really convenient!

#### Command History

You can easily access previous commands.  Hit the up arrow. Hit it again.  You can step backwards through your command history. The down arrow takes your forwards in the command history.

'Ctrl-r' will do a reverse-search through your command history.  This
is very useful.

You can also review your recent commands with the `history` command.  Just enter:

```bash
$ history
```

to see a numbered list of recent commands, including this just issues
`history` command. Only a certain number of commands are stored and displayed with `history`, there is a way to modify this to store a different number.

> **NOTE:** So far we have only run very short commands that have few or no arguments, and so it would be faster to just retype it than to check the history. However, as you start to run analyses on the commadn-line you will find your commands to be more complex and the history to be very useful!

**Other handy command-related shortcuts**

- <button>Ctrl + C</button> will cancel the command you are writing, and give you a fresh prompt.
- <button>Ctrl + A</button> will bring you to the start of the command you are writing.
- <button>Ctrl + E</button> will bring you to the end of the command.

## Commands, options, and keystrokes covered

```
bash
cd
ls
man
pwd
~           # home dir
.           # current dir
..          # parent dir
*           # wildcard
echo
ctrl + c    # cancel current command
ctrl + a    # start of line
ctrl + e    # end of line
history
cat
less
head
tail
cp
mdkir
mv
rm
```

#### Information on the shell

shell cheat sheets:<br>
* [http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/](http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/)
* [https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md](https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md)

Explain shell - a web site where you can see what the different components of
a shell command are doing.  
* [http://explainshell.com](http://explainshell.com)
* [http://www.commandlinefu.com](http://www.commandlinefu.com)

Data Carpentry tutorial: [Introduction to the Command Line for Genomics](https://datacarpentry.org/shell-genomics/)

General help:
- http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html
- man bash
- Google - if you don't know how to do something, try Googling it. Other people
have probably had the same question.
- Learn by doing. There's no real other way to learn this than by trying it
out.  Write your next paper in vim (really emacs or vi), open pdfs from
the command line, automate something you don't really need to automate.

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*
