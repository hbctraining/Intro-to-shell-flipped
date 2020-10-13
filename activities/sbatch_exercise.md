## Exercise

Convert existing script to a batch script and run it using the `sbatch` command from Slurm

### Set up
1. Copy the `generate_bad_reads_summary.sh` & create a new script called `sbatch_generate_bad_reads_summary.sh`.
2. Create a new directory inside the `raw_fastq` directory called `sbatch_output`.

### Update the new shell script
Using vim open up the file `sbatch_generate_bad_reads_summary.sh` to make some edits. Once inside the file, do the following:

> *Hint: Remember to change to insert mode in vim before beginning to edit!*

1. Update the lines of code shown below, such that the `grep` output in both cases is redirected to the `~/unix_lesson/raw_fastq/sbatch_output/` directory. *Do not change the names of the output files.*
      ```bash
      grep -A 2 -B 1 NNNNNNNNN $filename > ${samplename}-badreads.fq 
      
      grep -c -H  NNNNNNNNNN $filename >> badreads.count.summary
      ```
1. Add SLURM/`sbatch` directives at the top of the script requesting the following resources:
   * Use partition `priority` (-p)
   * Request 5 minutes (-t)
   * Request 100MB of memory (--mem)
   * Request a single core (-c)
   
1. Save the file and exit vim.

### Run the new shell script to start a new job on O2
1. Run the new script using the `sbatch` command

### Check the job/run 
1. Use `sacct` to check the status of your job submission
1. Check the contents of your current directory -
    * Are there any new files with names ending in `.out` and `.err`?
    * What are the contents of these two files?
1. Check the contents of `~/unix_lesson/raw_fastq/sbatch_output/`, are the expected files there?

---

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*
