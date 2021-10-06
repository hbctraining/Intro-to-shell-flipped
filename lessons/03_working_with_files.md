---
title: Working with files on the command-line"
author: "Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu, Will Gammerdinger"
date: "October 2021"
---

Approximate time: 30 min

## Learning Objectives

* Examine the contents of files
* Create a new file using the Vim text editor
* Execute basic operations using the Vim text editor

## Examining Files

We now know how to move around the file system and look at the contents of directories, but how do we look at the contents of files? On your laptop, viewing a file is as simple as finding it in the file explorer window and double clicking to open it. As you will have noticed so far, the point and click of the mouse is not very useful when working on the command-line. Instead we will need to equip ourseleves with some helpful commands.

### `cat` command

The easiest way to examine a file is to just print out all of its contents using the command `cat`. We can test this out by printing the contents of `~/unix_lesson/other/sequences.fa`. Enter the command followed by the filename, including the path when necessary:

```bash
$ cat ~/unix_lesson/other/sequences.fa
```

The `cat` command prints out the all the contents of `sequences.fa` to the screen.

> `cat` stands for catenate; it has many uses and printing the contents of a files onto the terminal is one of them.

**What does this file contain?**

```bash

>SRR014849.1 EIXKN4201CFU84 length=93 
GGGGGGGGGGGGGGGGCTTTTTTTGTTTGGAACCGAAAGGGTTTTGAATTTCAAACCCTTTTCGGTTTCCAACCTTCCAAAGCAATGCCAATA

>gi|340780744|ref|NC_015850.1| Acidithiobacillus caldus SM-1 chromosome, complete genome
ATGAGTAGTCATTCAGCGCCGACAGCGTTGCAAGATGGAGCCGCGCTGTGGTCCGCCCTATGCGTCCAACTGGAGCTCGTCACGAG
TCCGCAGCAGTTCAATACCTGGCTGCGGCCCCTGCGTGGCGAATTGCAGGGTCATGAGCTGCGCCTGCTCGCCCCCAATCCCTTCG
TCCGCGACTGGGTGCGTGAACGCATGGCCGAACTCGTCAAGGAACAGCTGCAGCGGATCGCTCCGGGTTTTGAGCTGGTCTTCGCT
CTGGACGAAGAGGCAGCAGCGGCGACATCGGCACCGACCGCGAGCATTGCGCCCGAGCGCAGCAGCGCACCCGGTGGTCACCGCCT
CAACCCAGCCTTCAACTTCCAGTCCTACGTCGAAGGGAAGTCCAATCAGCTCGCCCTGGCGGCAGCCCGCCAGGTTGCCCAGCATC
CAGGCAAATCCTACAACCCACTGTACATTTATGGTGGTGTGGGCCTCGGCAAGACGCACCTCATGCAGGCCGTGGGCAACGATATC
CTGCAGCGGCAACCCGAGGCCAAGGTGCTCTATATCAGCTCCGAAGGCTTCATCATGGATATGGTGCGCTCGCTGCAACACAATAC
CATCAACGACTTCAAACAGCGTTATCGCAAGCTGGACGCCCTGCTCATCGACGACATCCAGTTCTTTGCGGGCAAGGACCGCACCC

>gi|129295|sp|P01013|OVAX_CHICK GENE X PROTEIN (OVALBUMIN-RELATED)
QIKDLLVSSSTDLDTTLVLVNAIYFKGMWKTAFNAEDTREMPFHVTKQESKPVQMMCMNNSFNVATLPAE

```

### `less` command

`cat` is a terrific command, but when the file is really big, it can be annoying to use. In practice, when you are running your analyses on the command-line you will most likely be dealing with large files. In our case, we have FASTQ files. Let's take a look at the list of raw_fastq files and add the `-h` modifier to see how big the files are. 

```bash
$ ls -lh ~/unix_lesson/raw_fastq
```

> The `ls` command has a modifier `-h` when paired with `-l`, will list the files and also print sizes of files in human readable format. 

In the fourth column you wll see the size of each of these files, and you can see they are quite large, so we probably do not want to use the `cat` command to look at them. Instead, we can use the `less` command. 

Move into our `raw_fastq` directory and enter the following command:

```bash
$ less Mov10_oe_1.subset.fq
```
Rather than printing to screen, the `less` command opens the file in a new buffer allowing you to navigate through it. Does this look familiar? You might remember encountering a similar interface when you used the `man` command. This is because `man` is using the `less` command to open up the documentation files! The keys used to move around the file are identical to the `man` command. Below we have listed some additional shortcut keys for naviagting through your file when using `less`.

<span class="caption">Shortcuts for `less`</span>

| key              | action                 |
| ---------------- | ---------------------- |
| <kbd>SPACE</kbd> | to go forward          |
| <kbd>b</kbd>     | to go backwards        |
| <kbd>g</kbd>     | to go to the beginning |
| <kbd>G</kbd>     | to go to the end       |
| <kbd>q</kbd>     | to quit                |


Use the shortcut keys to move through your FASTQ file, we will explore these files in more detail later in the workshop. 

#### Searching files with `less`

`less` also gives you a way of searching through files. 

Just type in <kbd>/</kbd> to begin a search, you will see that the `/` will show up at the  bottom of the `less` buffer. Now, enter the name of the string of characters you would like to search for and hit the enter key. The interface will move to show you the location where that string is found, and highlight the string. If you hit <kbd>/</kbd> then <kbd>ENTER</kbd>, `less` will just repeat the previous search. 

`less` searches from the current location and works its way forward. For instance, let's search for the sequence `GAGACCC` in our file. You can see that we go right to that sequence and can see what it looks like. 

If you start a search when you are at the end of the file, `less` will not find it. You need to go to the beginning of the file and search.

To exit hit <kbd>q</kbd>. There are other more sophisticated commands to search through your file (and we will cover these later), but this shortcut search is useful for a quick scan through. You can think of it as being analagous to using the <kbd>Ctrl-F</kbd> keystroke when searching on your laptop.

### `head` and `tail` commands

There's another way that we can look at files, and just look at part of them. In particular, if we just want to see the beginning or end of the file to see how it's formatted.

The commands are `head` and `tail` and they just let you look at the beginning and end of a file respectively.

```bash
$ head Mov10_oe_1.subset.fq
```

```bash
$ tail Mov10_oe_1.subset.fq
```

By default, the first or last 10 lines will be printed to screen. The `-n` option can be used with either of these commands to specify the number `n` lines of a file to display. For example, let's print the first/last line of the file:

```bash
$ head -n 1 Mov10_oe_1.subset.fq

$ tail -n 1 Mov10_oe_1.subset.fq
```

*** 

**Exercise**

1. Change directories into `genomics_data`. You can do this using a full or relative path.
2. Use the `less` command to open up the file `Encode-hesc-Nanog.bed`.
3. Search for the string `chr11`; you'll see all instances in the file highlighted.
4. Staying in the `less` buffer, use the shortcut to get to the end of the file. Report the three highlighted lines at the end of the file where you see `chr11` highlighted. 
5. Exit the `less` buffer and come back to the command prompt.
6. Print to screen the last 5 lines of the file `Encode-hesc-Nanog.bed`. Report what you see as the output within the Terminal.

***

## Writing files

We've been able to do a lot of work with files that already exist, but what if we want to write and/or create our own files? Obviously, we're not going to type in sequence information for a FASTA file, but you'll see as we go that there are a lot of situations in which we would need to write/create a file or edit an existing file.

In order to create or edit files we will need to use a **text editor**. When we say, "text editor," we really do mean "text": these editors can
only work with plain character data, not tables, images, or any other media. The types of text editors available can generally be grouped into two categories: **graphical user interface (GUI) text editors** and **command-line editors**.

#### GUI text editors

A GUI is an interface that has buttons and menus that you can click on to issue commands to the computer and you can move about the interface just by pointing and clicking. You might be familar with GUI text editors, such as [BBEdit](http://www.barebones.com/products/bbedit/), [Sublime](http://www.sublimetext.com/), and [Notepad++](http://notepad-plus-plus.org/), which allow you to write and edit plain text documents. These editors often have features to easily search text, extract text, and highlight syntax from multiple programming languages. They are great tools, but since they are 'point-and-click', we cannot efficiently use them from the command line.

#### Command-line editors

When working remotely, we need a text editor that functions from the command line interface. With command-line editors you must navigate the interface using the arrow keys and shortcuts, since you do not have the option to 'point-and-click'. Some popular editors include [Emacs](http://www.gnu.org/software/emacs/), [Vim](http://www.vim.org/), or a graphical editor such as [Gedit](http://projects.gnome.org/gedit/). These are editors which are generally available for use on high-performance compute clusters. There are also simpler editors available for use on the cluster (e.g. [nano](http://www.nano-editor.org/)), but tend to have limited functionality.

### Introduction to Vim 

To write and edit files, we're going to use a text editor called 'Vim'. Vim is a very powerful text editor, and it offers extensive text editing options. However, in this introduction we are going to focus on **exploring some of the more basic functions**. 

> #### How do I keep track of all these shortcuts in Vim?
> To help you remember some of the keyboard shortcuts that are introduced below and to allow you to explore additional functionality on your own, we have compiled a [cheatsheet linked here](https://github.com/hbctraining/In-depth-NGS-Data-Analysis-Course/blob/master/resources/VI_CommandReference.pdf). Download it to your computer, it is a useful resource to have open while using Vim.
> <p align="center">
> <img src="../img/vim_cheatsheet.png" width="600">
> </p>


### Vim Interface

You can create a document by calling a text editor (in our case `vim`) and providing the name of the document you wish to create. 

Change directories to the `~/unix_lesson/other` folder and create a document using called `draft.txt` using the `vim` command:

```bash
$ cd ~/unix_lesson/other
	
$ vim draft.txt
```

**Note the `"draft.txt" [New File]` typed at the bottom left-hand section of the screen.** This tells you that you just created a new file in vim. 


### Vim Modes
Vim has **_two basic modes_** that will allow you to create documents and edit your text:   

- **_command mode (default mode):_** will allow you to save and quit the program (and execute other more advanced commands).  

- **_insert (or edit) mode:_** will allow you to write and edit text


Upon creation of a file, vim is automatically in command mode. Let's _change to insert mode_ by typing <kbd>i</kbd>. **Note the `--INSERT--` at the bottom left hand of the screen.** Now type in a few lines of text:

<p align="center">
<img src="../img/vim_insert.png" width="600">
</p>

After you have finished typing, **press <kbd>esc</kbd> to enter command mode.** 

**Note the `--INSERT--` has now disappeared from the bottom of the screen.**

> ### Review of Vim modes
> 
> | key              | action                 |
> | ---------------- | ---------------------- |
> | <button>i</button>     | insert mode - to write and edit text |
> | <button>esc</button>     | command mode - to issue commands / shortcuts  |
> 


### Saving and Quitting

To **"write to file"** or save the modifications made to the file, **type <kbd>:w</kbd>** when in command mode. You can see the commands you type in the bottom left-hand corner of the screen. 

<p align="center">
<img src="../img/vim_save.png" width="600">
</p>

After you have saved the file, the total number of lines and characters in the file will print out at the bottom left-hand section of the screen.

<p align="center">
<img src="../img/vim_postsave.png" width="600">
</p>


Alternatively, we can **write to file (save changes) and quit** all at once by **typing <kbd>:wq</kbd>**. Now, you should have exited vim and returned back to your command prompt.

To edit the newly created `draft.txt` file, you can open it again with vim: `vim draft.txt`. First, change to *insert mode* and type a few additional lines (you can move around the lines using the arrows on the keyboard). This time we decide to **quit without saving** by going into command mode by pressing the <button>esc</button> key, and then **typing <kbd>:q!</kbd>**.
 
<p align="center">
<img src="../img/vim_quit.png" width="600">
</p>


> ### Review of saving and quitting
> 
> | key (in command mode)  | action           |
> | ---------------- | ---------------------- |
> | <button>:w</button>     | to write to file (save) |
> | <button>:wq</button>     | to write to file and quit |
> | <button>:q!</button>     | to quit without saving |


### Shortcuts in Vim

While we cannot point and click to navigate the document, we can use the arrow keys to move around. However, navigating with arrow keys can be very slow, so Vim has shortcuts (which are completely unintuitive, but very useful as you get used to them over time). 

Create a new file called `spider.txt` using `vim`. Go into *insert mode* and enter the text as shown below in the screenshot: 

<p align="center">
<img src="../img/vim_spider.png" width="600">
</p>

Once you have finished typing, you can display line numbers by changing to *command mode* and then typing the <kbd>:set number</kbd> command. Later, if you choose to remove the line numbers you can reset it with <kbd>:set nonumber</kbd>.

<p align="center">
<img src="../img/vim_spider_number.png" width="600">
</p>

| key (in command mode)  | action                 |
| ---------------- | ---------------------- |
| <button>:set number</button>     | to number lines |
| <button>:set nonumber</button>     | to remove line numbers    |


**Save the document.** Check to see what mode you are currently in. **While in command mode**, try moving around the file `spider.txt` and familarizing yourself with some of these shortcuts!  

**Navigating around the file**

| key (in command mode) | action                 |
| ---------------- | ---------------------- |
| <button>gg</button>     | to move to top of file |
| <button>G</button>     | to move to bottom of file     |
| <button>$</button>     | to move to end of line |
| <button>0</button>     | to move to beginning of line     |
| <button>w</button>     | to move to next word     |
| <button>b</button>     | to move to previous word     |

Practice some of the editing shortcuts, then **quit the document without saving any changes**.

**Editing the file**

| key (in command mode) | action                 |
| ---------------- | ---------------------- |
| <button>dw</button>     | to delete word |
| <button>dd</button>     | to delete line     |
| <button>u</button>     | to undo |
| <button>Ctrl + r</button>     | to redo     |
| <button>/*pattern*</button>     | to search for a pattern (*n/N* to move to next/previous match)    |
| <button>:%s/*search*/*replace*/g</button>     | to search for a pattern and replace for all occurences     |


*** 

**Exercise**

We have covered some basic commands in vim, but practice is key for getting comfortable with the program. Let's
practice what we just learned in a brief challenge.

1. Open `spider.txt`, and delete the word "water" from line #2.
2. Quit without saving.
3. Open `spider.txt` again, and replace every occurrence of "spider" with "unicorn".
4. Delete: "Down came the rain." 
5. Save the file.
6. Undo your previous deletion.
7. Redo your previous deletion.
8. Delete the first and last words from each of the lines.
9. Save the file.
10. Open up the file and copy and paste the contents to a text editor on your local laptop to submit as homework.


---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*
