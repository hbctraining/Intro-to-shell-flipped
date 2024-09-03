# Workshop Schedule

## Day 1

| Time |  Topic  | Instructor |
|:-----------:|:----------:|:--------:|
| 9:30 - 10:10 | [Workshop introduction](../lectures/workshop_intro_slides.pdf) | Will |
| 10:10 - 11:40 | [Introduction to Shell](../lessons/01_the_filesystem.md) | Upen |
| 11:40 - 12:00 | Overview of self-learning materials and homework submission | Will |

### Before the next class:

I. Please **study the contents** and **work through all the code** within the following lessons:

  1. [Wildcards and shortcuts in Shell](../lessons/02_wildcards_shortcuts.md)
      <details>
       <summary><i>Click here for a preview of this lesson</i></summary>
         <br>Perhaps you are interested in only listing the files that have a <code>.txt</code> extension or you want to navigate to your home directory quickly. There are many shortcuts in Shell that will help you do these types of tasks. <br><br>This lesson will cover:<br>
             - Utilizing wildcards for selecting multiple files<br>
             - Implementing shortcuts for moving around the Shell quickly<br><br>
        </details>
        
  2. [Examining and creating files](../lessons/03_working_with_files.md)
      <details>
       <summary><i>Click here for a preview of this lesson</i></summary>
         <br>Now that you can navigate around the Shell environment, you are likely interested to know how to view and edit your files.<br><br>This lesson will cover:<br>
             - Viewing your files<br>
             - Editing your files using <code>vim</code><br><br>
        </details>
        
  3. [Searching and redirection](../lessons/04_searching_files.md)
      <details>
       <summary><i>Click here for a preview of this lesson</i></summary>
         <br>You will encounter large files that need a search function to find the information you are looking for. You might also be interested in writing the output of that search to a file or use it as the input to another function.<br><br>This lesson will cover:<br>
             - Searching files using <code>grep</code><br>
             - Writing the output of a command to a file<br>
             - Redirecting the output of a command to an additional command<br><br>
        </details>

> **NOTE:** To run through the code above, you will need to be **logged into O2** and **working on a compute node** (i.e. your command prompt should have the word `compute` in it).
> 1. Log in using `ssh rc_trainingXX@o2.hms.harvard.edu` and enter your password (replace the "XX" in the username with the number you were [assigned in class](https://docs.google.com/spreadsheets/d/1kBlYowhjjHJC9ZovmbBULmbqozKpprM17vZ2wPlhNg0/edit?usp=sharing)). 
> 2. Once you are on the login node, use `srun --pty -p interactive -t 0-2:30 --mem 1G /bin/bash` to get on a compute node.
> 3. Proceed once your command prompt has the word `compute` in it.
> 4. If you log out between lessons (using the `exit` command twice), please follow points 1. and 2. above to log back in and get on a compute node when you restart with the self learning.

II. **Complete the exercises**:
   * Each lesson above contains exercises; please go through each of them.
   * **Copy over** your solutions into the [Google Form](https://docs.google.com/forms/d/e/1FAIpQLSdrIjkOw6-vNmgDMlKQml49yFFEBKX1Z63dmSUyp_49-xeMCQ/viewform?usp=sf_link) the **day before the next class**.

### Questions?
* ***If you get stuck due to an error*** while runnning code in the lesson, [email us](mailto:hbctraining@hsph.harvard.edu) 

***

## Day 2

| Time |  Topic  | Instructor |
|:-----------:|:----------:|:--------:|
| 09:30 - 10:10 | Self-learning lessons review | All |
| 10:10 - 10:55 | [Shell scripts and variables in Shell](../lessons/05_shell-scripts_variable.md)| Upen |
| 10:55 - 12:00 | [Loops and automation](../lessons/06_loops_and_automation.md) | Will |


### Before the next class:

I. Please **study the contents** and **work through all the code** within the following lessons:

1. [Permissions and Environment Variables](../lessons/07_permissions_and_environment_variables.md)

      <details>
       <summary><i>Click here for a preview of this lesson</i></summary>
         <br>When using a multi-user system like the O2 cluster, you may want to limit access to your work. Permissions exist to clearly delineate who has the ability to read, write and execute your files.<br><br>
         Also, when working in a UNIX system, there are a core set of default variables that control the behavior of your command-line. One of the most important of these is the $PATH variable, which tells the system where to look for commands that you give it. <br><br>This lesson will cover:<br>
             - Interpreting and modifying existing permissions<br>
             - Querying environmental variables<br>
             - Reading and appending to the $PATH variable<br><br>
        </details>

2. [Introduction to High-performance computing](../lessons/08_HPC_intro_and_terms.md)

      <details>
       <summary><i>Click here for a preview of this lesson</i></summary>
         <br>Now that you had a chance to explore the O2 cluster, let's focus on the components of this system, how it is different than your personal computer and the advantages that it offers in terms of parallelization. <br><br>This lesson will cover:<br>
             - Differentiating a high-performance computing cluster like O2 from your personal computer<br>
             - Discuss the large parallelization advantage that O2 has over a personal computer<br><br>
        </details>

> **NOTE:** To run through the code above, you will need to be **logged into O2** and **working on a compute node** (i.e. your command prompt should have the word `compute` in it). For login instructions, please see above.

II. **Complete the exercises**:
   * Each lesson above contains exercises; please go through each of them.
   * **Copy over** your solutions into the [Google Form](https://docs.google.com/forms/d/e/1FAIpQLScvw7yahfXZSB1jJZsGNLIzTf2Ip7JWj9G5Eh6_ycMuUYK-Kw/viewform?usp=sf_link) the **day before the next class**.
   
### Questions?
* ***If you get stuck due to an error*** while runnning code in the lesson, [email us](mailto:hbctraining@hsph.harvard.edu) 

***

## Day 3

| Time |  Topic  | Instructor |
|:-----------:|:----------:|:--------:|
| 9:30 - 10:00 | Self-learning lessons review | All |
| 10:00 - 11:00 | [Introduction to the O2 cluster](../lectures/HPC_intro_O2_May2024.pdf) | Will |
| 11:00 - 11:30 | [Exercise](../activities/sbatch_exercise.md) ([answer key](https://raw.githubusercontent.com/hbctraining/Intro-to-shell-flipped/master/activities/sbatch_exercise_answer.txt))| Upen |
| 11:30 - 11:45 | Introduction to the O2 cluster - data storage| Will |
| 11:45 - 12:00 | [Wrap up](../lectures/workshop_wrapup_slides.pdf) | Upen |

***

## Dataset
[Introduction to Shell: Dataset](https://www.dropbox.com/s/2u4lk3d10yuio0r/unix_lesson.zip?dl=1)

## Answer keys
* [Day 1 Exercises](../homework/Day1_answer_key.txt)
* [Day 2 Exercises](../homework/Day2_answer_key.txt)


## Advanced bash commands
If you are interested in learning some more advanced tools for working on the command-line, we encourage you to walk-through the materials linked below:

* [More fun with bash commands](../lessons/extra_bash_tools.md)
* [Advanced bash commands (aliases, copying files, and symlinks)](https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon-flipped/lessons/more_bash_cluster.html)

## Resources

Cheat sheets:
* [http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/](http://fosswire.com/post/2007/08/unixlinux-command-cheat-sheet/)
* [https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md](https://github.com/swcarpentry/boot-camps/blob/master/shell/shell_cheatsheet.md)
* [tldr_ : Simplified version of the `man` pages (online and searchable)](https://tldr.ostera.io/)

Online tutorials:
* [Explain Shell](http://explainshell.com)
* [Introduction to the Command Line for Genomics](https://datacarpentry.org/shell-genomics/)
* [BASH Programming - Introduction HOW-TO](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)
* [Bioinformatics from the Command Line](https://medium.com/ngs-sh)

General help:
* Google it! - if you don't know how to do something, try Googling it, other people have probably had the same question.
* Learn by doing! There's no real other way to learn this than by trying it out.
  * Use vim on your laptop
  * Move around the directory structure on your laptop using the Terminal/Shell counts
  * Open folders and files using the command `open`
  * Automate something you don't really need to automate
* Use `man bash` to get more information about bash (bourne-again shell)

***
*These materials have been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *Some materials used in these lessons were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
