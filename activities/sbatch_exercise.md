## Exercise

Convert existing script to a batch script and run it using the `sbatch` command from Slurm

### Set up
1. Copy the `generate_bad_reads_summary.sh` & create a new script called `sbatch_generate_bad_reads_summary.sh
2. Create a new directory within the `raw_fastq` directory called `sbatch_output`

### Update the new shell script
Next, open the `sbatch_generate_bad_reads_summary.sh` and do the following:
1. Update the following lines of code, sych that the `gerp` output in both cases is redirected to the `~/unix_lesson/raw_fastq/sbatch_output/` directory. *Do not change the names of the output files.*
      ```bash
      grep -A 2 -B 1 NNNNNNNNN $filename > ${samplename}-badreads.fq 
      
      grep -c -H  NNNNNNNNNN $filename >> badreads.count.summary
      ```
1. Add SLURM directives at the top of the script requesting the following resources:
   * Use partition `priority` (-p)
   * Request 5 minutes (-t)
   * Request 100MB of memory (--mem)
   * Request a single core (-c)

### Run the new shell script to start a new job on O2
1. Run the new script using the `sbatch` command

### Checking the job (during and after) 
1. Use `sacct` to check the status of your job submission
1. Check the contents of your current directory -
    * Are there any new files with names ending in `.out` and `.err`?
    * What are the contents of these two files?
1. Check the contents of `~/unix_lesson/raw_fastq/sbatch_output/`, are the expected files there?
