---
title: "The Shell: Searching and Redirection"
author: "Sheldon  McKay, Bob Freeman, Mary Piper, Radhika Khetani, Meeta Mistry, Jihe Liu, Will Gammerdinger"
date: "October 2021"
---

Approximate time: 60 minutes

## Learning objectives

- Search for characters or patterns in a text file using the `grep` command
- Write to and append a file using output redirection
- Use the pipe (`|`) character to chain together commands

## Searching files with `grep` command

We went over how to search within a file using `less`. We can also search within files without even opening them, using `grep`. Simply put `grep` is a command-line utility for searching plain-text data sets for lines matching a pattern or **reg**ular **ex**pression (regex). 

> Why the word "grep"? It is a shortened form of **g**lobally search for a **r**egular **e**xpression and **p**rint matching lines (g/re/p).

The syntax for `grep` is as follows: `grep  search_term  filename`. The pattern that we want to search is specified in `search_term` slot, and the file we want to search within is specified in the `filename` slot. Let's give it a try by searching the FASTQ files in the `raw_fastq` directory. 

FASTQ files contain the sequencing reads (nucleotide sequences) output from a sequencing facility. Each sequencing read in a FASTQ file is associated with four lines, with the first line (header line) always starting with an `@` symbol. A whole FASTQ record for a single read should appear similar to the following:

	@HWI-ST330:304:H045HADXX:1:1101:1111:61397
	CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
	+
	B?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################

> **More information about the FASTQ file format**
>
> |Line|Description|
> |----|-----------|
> |1|Read name preceded by '@'|
> |2|The actual DNA sequence|
> |3|Read name (same as line 1) preceded by a '+' or just a '+' sign|
> |4|String of characters which represent the quality score of each nucleotide in line 2; must have same number of characters as line 2|
>
> You can find more information about FASTQ files in [this lesson](https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon-flipped/lessons/05_qc_running_fastqc_interactively.html) from our [RNA-seq workshop](https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon-flipped/).

Suppose we want to see how many reads in our file `Mov10_oe_1.subset.fq` contain "bad" data, i.e. reads with 10 consecutive Ns (`NNNNNNNNNN`).

```bash
$ cd ~/unix_lesson/raw_fastq

$ grep NNNNNNNNNN Mov10_oe_1.subset.fq
```

We get back a lot of reads or lines of text!  

What if we wanted to see the whole FASTQ record for each of these reads? We would need to modify the default behavior of `grep` and specify some argument/options. To look for all available options for the `grep` command, we can type `grep --help` (or `man grep`). 

Looks like the `-B` and `-A` arguments for grep will be useful to return the matched line plus one before (`-B 1`) and two lines after (`-A 2`). Since each record is four lines, using these arguments will return the whole record. Within the whole record, the second line will be the actual sequence that has the pattern we searched for.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq
```

```
@HWI-ST330:304:H045HADXX:1:1101:1111:61397
CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
+
@?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################
--
@HWI-ST330:304:H045HADXX:1:1101:1106:89824
CACAAATCGGCTCAGGAGGCTTGTAGAAAAGCTCAGCTTGACANNNNNNNNNNNNNNNNNGNGNACGAAACNNNNGNNNNNNNNNNNNNNNNNNNGTTGG
+
?@@DDDDDB1@?:E?;3A:1?9?E9?<?DGCDGBBDBF@;8DF#########################################################
```

***

**Exercises**

1. Search for the sequence CTCAATGAGCCA in `Mov10_oe_1.subset.fq`. How many sequences do you find?

2. In addition to finding the sequence, how can you modify the command so that your search also returns the name of the sequence?

3. If you want to search for that sequence in **all** Mov10 replicate fastq files, what command would you use?

	<details>
		<summary><b><i>Answers</i></b></summary>
		<p><i>Question 1</i><br>
		<code>grep CTCAATGAGCCA Mov10_oe_1.subset.fq</code><br>
		The output returns 5 sequences.</p>
		<p><i>Question 2</i><br>
			<code>grep -B 1 CTCAATGAGCCA Mov10_oe_1.subset.fq</code></p>
		<p><i>Question 3</i><br>
		<code>grep CTCAATGAGCCA Mov10*</code><br>
		The output returns 5 sequences for Mov10_oe_1 and 3 sequences for Mov10_oe_2.</p>
	</details>

***

## Redirection

When we use grep, the matching lines print to the Terminal (also called Standard Output or "stdout"). If the result of the `grep` search is a few lines, we can view them easily, but if the output is very long, the lines will just keep printing and we won't be able to see anything except the last few lines. You have experienced this when you searched for the pattern `NNNNNNNNNN`. How can we capture them instead?

We can do that with something called "redirection". The idea is that we're redirecting the output from the Terminal (all the stuff that went whizzing by) to something else. In this case, we want to save it to a file, so that we can look at it later.

### Redirecting with ">"

**The redirection command for writing something to file is `>`.**

Let's try it out and put all the sequences that contain 'NNNNNNNNNN' from the `Mov10_oe_1.subset.fq` into another file called `bad_reads.txt`.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq > bad_reads.txt
```

The prompt will go away for a little bit and then you will get it back, but nothing will be printed on the Terminal. But, you should have a new file called `bad_reads.txt`!

```bash
$ ls -l
```

Take a look at the file and see if it contains what you think it should. 

> NOTE: If we already had a file named `bad_reads.txt` in our directory, it would have overwritten it without any warning!

### Redirecting (and appending) with ">>"

**The redirection command for appending something to an existing file is `>>`.**

If we use `>>`, it will append to the existing content in a file, rather than overwrite it. This can be useful for saving more than one search. For example, the following command will append the bad reads from Mov10_oe_2 to the bad_reads.txt file that we just generated.
    
```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_2.subset.fq >> bad_reads.txt

$ ls -l
```

Did the size of the `bad_reads.txt` file change?

Since our `bad_reads.txt` file isn't a raw_fastq file, we should move it to a different location within our directory. Let's move it to the `other` folder using the command `mv`. 

```bash
$ mv bad_reads.txt ../other/
```

### Redirecting with pipes "|" (or piping)

**The redirection command for using the output of a command as input for a different command is `|`.**

**The pipe key** (<kbd>|</kbd>) is very likely not something you use very often (it is on the same key as the back slash (<kbd>\\</kbd>), right above the <button>Enter/Return</button> key). 

What `|` does is take the output from one command, e.g. the output from `grep` that went whizzing by and runs it through the command specified after it. When it was all whizzing by before, we wished we could just take a look at it! Maybe we could use `less` instead of the rapid scroll. Well, it turns out that we can! We can **pipe the output `grep` command** to `less` to slowly scroll through, or to `head` to just see the first few lines.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq | less
```

Now we can use the arrows to scroll up and down and use `q` to get out.

Or we could just take a glance to see what the output looks like.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq | head -n 5
```

Another thing we can also do is count the number of lines output by `grep`. 

The `wc` command stands for ***w**ord **c**ount*. This command counts the number of lines, words and characters in the text input given to it. The `-l` argument will only count the number of lines instead of counting everything.

```bash
$ grep NNNNNNNNNN Mov10_oe_1.subset.fq | wc -l
```

*Try it out without the `-l` to see the full output.* 

> **Tip** - Similar to `grep`, you can type `wc --help` or `man wc` to see all options.

**About Pipes:**
* The pipe is a very important/powerful concept in Shell
* You can string along as many commands together as you like

The philosophy behind the three redirection operators (`>`, `>>`, `|`) you have learned so far is that none of them by themselves do a lot. BUT when you start chaining them together, you can do some really powerful things really efficiently. 

**To be able to use the shell effectively, becoming proficient in the use of the pipe and redirection operators is essential.**

## Practice with searching and piping/redirection

Let's use the new commands in our toolkit and a few new ones to examine the "gene annotation" file, **chr1-hg19_genes.gtf**. We will be using this file to find the genomic coordinates of all known exons on chromosome 1.

```bash
$ cd ~/unix_lesson/reference_data/
```

### Introduction to the GTF file format

Let's explore our `chr1-hg19_genes.gtf` file a bit. What information does it contain?

```bash
$ less chr1-hg19_genes.gtf
```

	chr1    unknown exon    14362   14829   .       -       .       gene_id "WASH7P"; gene_name "WASH7P"; transcript_id "NR_024540"; tss_id "TSS7245";
	chr1    unknown exon    14970   15038   .       -       .       gene_id "WASH7P"; gene_name "WASH7P"; transcript_id "NR_024540"; tss_id "TSS7245";
	chr1    unknown exon    15796   15947   .       -       .       gene_id "WASH7P"; gene_name "WASH7P"; transcript_id "NR_024540"; tss_id "TSS7245";
	chr1    unknown exon    16607   16765   .       -       .       gene_id "WASH7P"; gene_name "WASH7P"; transcript_id "NR_024540"; tss_id "TSS7245";
	chr1    unknown exon    16858   17055   .       -       .       gene_id "WASH7P"; gene_name "WASH7P"; transcript_id "NR_024540"; tss_id "TSS7245";


> **The GTF (Gene Transfer Format) file** is a tab-delimited file with information arranged in a very specific manner, usually for NGS analysis. That information is specifically about all the various entities or features found in a genome; in this case, on chromosome 1. 
>
> For more information on this file format, check out the [Ensembl site](http://useast.ensembl.org/info/website/upload/gff.html). 

The columns in the **GTF file contain the genomic coordinates (location) of gene features (exon, start_codon, stop_codon, CDS) along with the associated gene_names, transcript_ids and protein_ids (p_id)**. 

Given our understanding of splice isoforms, we know that a given exon can be part of 2 or more different transcripts generated from the same gene. In a GTF file, this exon will be represented multiple times, once for each transcript (or splice isoform). For example, 

```bash
$ grep PLEKHN1 chr1-hg19_genes.gtf | head -n 5
```

This search returns two different transcripts of the same gene, NM_001160184 and NM_032129, that contain the same exon.

### Two new commands!

Before our practice, let's learn two new commands. 

* **`cut` is a command that extracts columns from files.** 

We will use `cut` with the `-f` argument to specify which specific fields or columns from the dataset we want to extract. Let's say we want to get the 1st column (chromosome number) and the 4th column (starting genomic position) from `chr1-hg19_genes.gtf` file, we can say:

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf  | head
```

> The `cut` command assumes our data columns are separated by tabs (i.e. tab-delimited). The `chr1-hg19_genes.gtf` is a tab-delimited file, so the default `cut` command works for us. However, data can be separated by other types of delimiters like "," or ";". If your data is not tab delimited, there is an argument you can add to your `cut` command, `-d` to specify the delimiter (e.g. `-d ","` with a .csv file).

* **`sort` is a command used to sort the contents of a file in a particular order.** It has arguments that let you pick which column to sort by (`-k`), what kind of sorting you want to do (numeric `n`) and also if the result of the sorting should only return unique (`-u`) values. These are just 2 of the many features of the sort command. 

Let's do a quick test of how the `-u` argument returns only unique lines (and remove duplicates).

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf | wc -l
```
*How many lines are returned to you?*

<details>
	<summary><b><i>Click here to check your output</i></b></summary>
	<p>Your command should have returned 76,767 lines.</p>
</details>

Now apply the `sort -u` command before counting the lines.

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf | sort -u | wc -l
```
*How many lines do you see now?*

<details>
	<summary><b><i>Click here to check your output</i></b></summary>
	<p>Your command should have returned 27,852 lines.</p>
</details>

***

**Exercises**

Now that we know what type of information is inside the GTF file, let's use the commands we have learned so far to answer a simple question about our data: **how many unique exons are present on chromosome 1 using `chr1-hg19_genes.gtf`?**

To determine the number of unique exons on chromosome 1, we are going to perform a series of steps as shown below. In this exercise, you need to figure out the command line for each step. 
	
1. Extract only the genomic coordinates of exon features
2. Subset dataset to only keep genomic coordinates
3. Remove duplicate exons
4. Count the total number of exons

Your end goal is to have a single line of code, wherein you have strung together multiple commands using the pipe operator. But, we recommend that you do it in a stepwise manner as detailed below.

#### 1. Extract only the genomic coordinates of exon features

We only want the exons (not CDS or start_codon features), so let's use `grep` to search for the word "exon". You should do sanity check on the first few lines of the output of `grep` by piping the result to the `head` command. ***Report the command you have at this stage.*** 

#### 2. Subset the extracted information from step 1 to only keep genomic coordinates

We will define the uniqueness of an exon by its genomic coordinates, both start and end. Therefore, from the step 1 output, we need to keep 4 columns (chr, start, stop, and strand) to find the total number of unique exons. The column numbers you want are 1, 4, 5, and 7. 

You can use `cut` to extract those columns from the output of step 1. ***Report the command you have at this stage.***

At this point, the first few lines should look like this:

	chr1	14362	14829	-
	chr1	14970	15038	-
	chr1	15796	15947	-
	chr1	16607	16765	-
	chr1	16858	17055	-

#### 3. Remove duplicate exons

Now, we need to remove those exons that show up multiple times for different transcripts. We can use the `sort` command with the `-u` option. ***Report the command you have at this stage.***

Do you see a change in how the sorting has changed? By default the `sort` command will sort and what you can't see here is that it has removed the duplicates. We will use step 4 to check if this step worked.

#### 4. Count the total number of exons

First, check how many lines we would have without using `sort -u` by piping the output to `wc -l`.

Now, to count how many unique exons are on chromosome 1, we will add back the `sort -u` and pipe the output to `wc -l`. Do you observe a difference in number of lines?

***Report the command you have at this stage and the number of lines you see with and without the `sort -u`.***

<details>
	<summary><b><i>Answers</i></b></summary>
	<p><i>Question 1</i><br>
	<code>grep exon chr1-hg19_genes.gtf | head</code><br></p>
	<p><i>Question 2</i><br>
	<code>grep exon chr1-hg19_genes.gtf | cut -f 1,4,5,7  | head</code><br></p>
	<p><i>Question 3</i><br>
	<code>grep exon chr1-hg19_genes.gtf | cut -f 1,4,5,7 | sort -u | head</code><br></p>
	<p><i>Question 4</i><br>
	<code>grep exon chr1-hg19_genes.gtf | cut -f 1,4,5,7 | wc -l</code><br>
	The output returns 37,213 lines.<br>
	<code>grep exon chr1-hg19_genes.gtf | cut -f 1,4,5,7 | sort -u | wc -l</code><br>
	The output returns 22,769 lines, indicating that repetitive lines have been removed.<br>
</details>

### Summary!

For what we did in one line of code in the exercise at the end, we could have done it in multiple steps by saving the output of each command to a new file. But that would not be as efficient as using pipes! All we needed was the number of unique exons, and the intermediate output was not useful. Avoiding the storage of the data from each of the intermediate steps prevents clutter, and helps us avoid wasting precious storage space! 

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

* *Adapted from the lesson by Tracy Teal. Contributors: Paul Wilson, Milad Fatenejad, Sasha Wood, and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*

