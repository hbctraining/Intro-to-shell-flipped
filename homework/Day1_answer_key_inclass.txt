# Day 1 in-class Exercises

## The Unix directory file structure

1. Using one command move to your home directory.
 $cd

2. Using one command list the contents of the reference_data directory that is within the unix_lesson directory.
 $ls -l ~/unix_lesson/reference_data

3. Using one command list one of the files in reference_data.
 $ls -l ~/unix_lesson/reference_data/Irrel_kd_1.subset.fq

## Copying, creating, moving and removing data

1. Create a new folder in unix_lesson called selected_fastq
 $cd ~/unix_lesson
 $mkdir selected_fastq

2. Copy over the Irrel_kd_2.subset.fq and Mov10_oe_2.subset.fq from raw_fastq to the ~/unix_lesson/selected_fastq folder
 $cp ~/unix_lesson/raw_fastq/Irrel_kd_2.subset.fq ~/unix_lesson/selected_fastq
 $cp ~/unix_lesson/raw_fastq/Mov10_oe_2.subset.fq ~/unix_lesson/selected_fastq

3. Rename the selected_fastq folder and call it exercise1
 $mv selected_fastq exercise1
