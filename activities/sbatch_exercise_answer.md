#!/bin/bash

#SBATCH -p priority
#SBATCH -c 1
#SBATCH -t 0-00:05:00
#SBATCH --mem 100MB

# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq

# count bad reads for each FASTQ file in our directory
for filename in *.fq 
do 

  # create a prefix for all output files
  samplename=`basename $filename .subset.fq`

  # tell us what file we're working on	
  echo $filename

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/sbatch_output/${samplename}_badreads.fq

  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/sbatch_output/badreads.count.summary
done
