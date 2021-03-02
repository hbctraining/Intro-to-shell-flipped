## Exercise

Convert existing script to a batch script and run it using the `sbatch` command from Slurm

### Set up
1. Locate the file `generate_bad_reads_summary.sh` (which was created in the [Loops and Automation lesson](../lessons/06_loops_and_automation.md))
> *Hint: This file is likely in your `badreads` directory.*
```bash
cd ~/unix_lesson/badreads
```

2. Duplicate this file and call the duplicate file `sbatch_generate_bad_reads_summary.sh`. (*Hint: use the `cp` command*)
```bash
cp generate_bad_reads_summary.sh sbatch_generate_bad_reads_summary.sh
```

3. Create a new directory inside the `badreads` directory called `sbatch_output`.
```bash
mkdir sbatch_output
```

### Update the new shell script
Using vim open up the file `sbatch_generate_bad_reads_summary.sh` to make some edits. Once inside the file, do the following:

> *Hint: Remember to change to insert mode in vim before beginning to edit!*

1. Update these two lines of code below, such that the `grep` output in both cases is redirected to the `~/unix_lesson/badreads/sbatch_output/` directory. *Do not change the names of the output files.*
      ```bash
          grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/${samplename}_badreads.fq
          
          grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/badreads.count.summary
      ```
1. Add SLURM/`sbatch` directives at the top of the script requesting the following resources:
   * Use partition `priority` (`-p`)
   * Request 5 minutes (`-t`)
   * Request 100MB of memory (`--mem`)
   * Request a single core (`-c`)
   
1. Save the file and exit vim.

### Run the new shell script to start a new job on O2
1. Run the new script using the `sbatch` command

### Check the job/run 
1. Use `sacct` to check the status of your job submission
1. Check the contents of your current directory -
    * Are there any new files with names ending in `.out` and `.err`?
    * What are the contents of these two files?
1. Check the contents of `~/unix_lesson/badreads/sbatch_output/`, are the expected files there?

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*
