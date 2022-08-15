# Workshop Schedule

| Time |  Topic  | Instructor |
|:-----------:|:----------:|:--------:|
| 1:30 - 2:30 | [Introduction to the O2 cluster](../lectures/HPC_intro_O2_July2022.pdf)  | Radhika |
| 2:30 - 3:00 | [Exercise](../activities/sbatch_exercise.md) ([answer key](https://raw.githubusercontent.com/hbctraining/Intro-to-shell-flipped/master/activities/sbatch_exercise_answer.txt)) | Will |

## Before we start

1. Log in using `ssh rc_trainingXX@o2.hms.harvard.edu` and enter your password (replacing the "XX" in the username with the number you are [assigned](https://docs.google.com/spreadsheets/d/1kBlYowhjjHJC9ZovmbBULmbqozKpprM17vZ2wPlhNg0/edit?usp=sharing)). 
2. Once you are on the login node, use `srun --pty -p interactive -t 0-2:00 --mem 1G /bin/bash` to get on a compute node.
3. Copy the `unix_lesson` directory to your home directory using the following command: `cp -r /n/groups/hbctraining/unix_lesson_sbatch/ ~/unix_lesson/` 

***
*These materials have been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *Some materials used in these lessons were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

