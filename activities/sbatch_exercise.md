## Exercise

Convert existing script to a batch script and run it using

1. Copy `generate_bad_reads_summary.sh` & create a new script called `generate_bad_reads_summary_sbatch.sh`
1. Open `generate_bad_reads_summary_sbatch.sh` and add some SLURM directives at the beginning of the script, as follows:
   * Use partition `priority` (-p)
   * Request 5 minutes (-t)
   * Request 100MB of memory (--mem)
   * Request a single core (-c)
1. Run the new script using the `sbatch` command
1. Use `sacct` to check the status of your job submission
