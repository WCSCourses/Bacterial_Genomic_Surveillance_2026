<img src="https://coursesandconferences.wellcomeconnectingscience.org/wp-content/themes/wcc_courses_and_conferences/dist/assets/svg/logo.svg" width="200" height="200">

[<<< Go back to Manual Contents Page](https://github.com/WCSCourses/GenEpiLAC2025/blob/main/course_modules_2025/Manual_main.md)

<br>

# Module 1 - Sequencing & QC - Costa Rica 2025 <!-- omit in toc -->

## Table of contents <!-- omit in toc -->
- [Introduction](#introduction)
  - [Brief recap](#brief-recap)
  - [Commonly used file formats for NGS data](#commonly-used-file-formats-for-NGS-data)
    - [FASTQ](#fastq)
    - [FASTA](#fasta)
    - [SAM and BAM](#sam-and-bam)
  - [Quality control for FastQ data](#quality-control-for-FastQ-data)
- [Practical Exercise](#practical-exercise)
  - [QC of short data](#QC-of-short-data)
  - [QC of long data](#QC-of-long-data)
  - [Bonus](#bonus)
  - [Wrap up questions](#Wrap-up-questions)

## Module Aims

1. Perform quality control (QC) assessments on FASTQ-formatted sequence data obtained from both short and long reads sequencing technologies.
2. Identify possible contamination in high throughput sequence data.
3. Critically evaluate and interpret QC outputs to determine data reliability.

# [Introduction](#introduction)

## [Brief recap](#brief-recap)

Before we start, during the whole week, you will be using most of the concepts you´ve learnt in the pre-course "Introduction to Linux for biologists". Below you can find a list of frequently used commands for you to remember them: 

| Command | What it does |
| ------ | ------------ |
| ls | Lists the contents of the current directory  |
| mkdir  | Makes a new directory  |
| mv | Moves or renames a file  |
| cp  | Copies a file  |
| rm | Removes a file  |
| mkdir  | Makes a new directory  |
| cat | Concatenates files  |
| more  | Displays the contents of a file one page at a time  |
| head | Displays the first ten lines of a file  |
| tail  | Displays the last ten lines of a file  |
| cd | Changes current working directory  |
| pwd	 | Prints working directory  |
| find  | Finds files matching an expression  |
| grep | Searches a file for patterns  |
| wc  | Counts the lines, words, characters, and bytes in a file  |
| kill | Stops a process  |
| jobs | Lists the processes that are running  |

```
Command line shortcuts 
●	Up/Down arrows: Previous commands 
●	!!: Reruns previous command 
●	Tab: Auto complete 
●	Tab+Tab: All available options 
●	Ctrl+a: Move cursor to start of line 
●	Ctrl+e: Move cursor to end of line 
●	Alt+: Alternates between terminals 
●	Ctrl+l: Clear screen 
●	Ctrl+c: Terminates the running program 
●	Ctrl+z: Suspends the running program 
●	Ctrl+w: Removes a previous word 
●	Ctrl+d: Logout 
●	Ctrl+d(in a command): Removes a character 
●	Ctrl+u: Removes till the beginning 
```
## [Commonly used file formats for NGS data](#commonly-used-file-formats-for-NGS-data)

### FASTA

Among the most common and simplest file formats for representing nucleotide sequences is FASTA.  Essentially, each sequence is represented by a 'header' line that begins with a '>', followed by lines containing the actual nucleotide sequence. By convention, the first 'word' in the header line is a unique identifier, which is usually the accession number. Consider this portion of the whole genome of *Staphylococcus aureus* MRSA252 as an example of a FASTA-formatted nucleotide sequence:

```
  >NC_002952.2 Staphylococcus aureus subsp. aureus MRSA252, complete sequence
  CGATTAAAGATAGAAATACACGATGCGAGCAATCAAATTTCATAACATCACCATGAGTTTGATCCAAAGCATGAGTGTTT
  ACAATGTTTGAATACCTTATACAGTTCTTATACATACTTTATAAATTATTTCCCAAGCTGTTTTGATACACACACTAACA
  GATACTCTATAGAAGGAAAAGTTATCCACTTATGCACACTTATACTTTTTAGAATTGTGGATAATTAGAAATTACACACA
  AAGTTATACTATTTTTAGCAACATATTCACAGGTATTTGACATATAGAGAACTGAAAAAGTATAATTGTGTGGATAAGTC
  GTCCAACTCATGATTTTATAAGGATTTATTTATTGATATTTACATAAAAATACTGTGCATAACTAATAAGCAGGATAAAG
  TTATCCACCGATTGTTATTAACTTGTGGATAATTATTAACATGGTGTGTTTAGAAGTTATCCACGGTTGTTATTTTTGTG
  TATAACTTAAAAATTTAAGAAAGATGGAGTAAATTTATGTCGGAAAAAGAAATTTGGGAAAAAGTGCTTGAAATTGCTCA
  AGAAAAATTATCAGCTGTAAGTTACTCAACTTTCCTAAAAGATACTGAGCTTTACACGATCAAAGATGGTGAAGCTATCG
  TATTATCGAGTATTCCTTTTAATGCAAATTGGTTAAATCAACAATATGCTGAAATTATCCAAGCAATCTTATTTGATGTT
  GTAGGCTATGAAGTAAAACCTCACTTTATTACTACTGAAGAATTAGCAAATTATAGTAATAATGAAACTGCTACTCCAAA
  AGAAGCAACAAAACCTTCTACTGAAACAACTGAGGATAATCATGTGCTTGGTAGAGAGCAATTCAATGCCCATAACACAT
  TTGACACTTTTGTAATCGGACCTGGTAACCGCTTCCCACATGCAGCGAGTTTAGCTGTGGCCGAAGCACCAGCCAAAGCG
  TACAATCCATTATTTATCTATGGAGGTGTTGGTTTAGGAAAAACCCATTTAATGCATGCCATTGGTCATCATGTTTTAGA
  TAATAATCCAGATGCCAAAGTGATTTACACATCAAGTGAAAAATTCACAAATGAATTTATTAAATCAATTCGTGATAACG
```

- The first line begins with '>' indicating that it is the header line.
- This is immediately followed by 'NC_002952.2', which is the accession number for [this sequence in the NCBI Assembly database](https://www.ncbi.nlm.nih.gov/assembly/GCF_000011505.1).
- Then follows the actual nucleotide sequence, split over several lines.

It is very common to combine multiple sequences into a single multi-FASTA file like this:

```
>ctxB1
ATGATTAAATTAAAATTTGGTGTTTTTTTTACAGTTTTACTATCTTCAGCATATGCACAT
GGAACACCTCAAAATATTACTGATTTGTGTGCAGAATACCACAACACACAAATACATACG
CTAAATGATAAGATATTTTCGTATACAGAATCTCTAGCTGGAAAAAGAGAGATGGCTATC
ATTACTTTTAAGAATGGTGCAACTTTTCAAGTAGAAGTACCAGGTAGTCAACATATAGAT
TCACAAAAAAAAGCGATTGAAAGGATGAAGGATACCCTGAGGATTGCATATCTTACTGAA
GCTAAAGTCGAAAAGTTATGTGTATGGAATAATAAAACGCCTCATGCGATTGCCGCAATT
AGTATGGCAAATTAA
>ctxB10
ATGATTAAATTAAAATTTGGTGTTTTTTTTACAGTTTTACTATCTTCAGCATATGCACAT
GGAACACCTCAAAATATTACTGATTTGTGTGCAGAATACCCCAACACACAAATATATACG
CTAAATGATAAGATATTTTCGTATACAGAATCTCTAGCTGGAAAAAGAGAGATGGCTATC
ATTACTTTTAAGAATGGTGCAATTTTTCAAGTAGAAGTACCAGGTAGTCAACATATAGAT
TCACAAAAAAAAGCGATTGAAAGGATGAAGGATACCCTGAGGATTGCATATCTTACTGAA
GCTAAAGTCGAAAAGTTATGTGTATGGAATAATAAAACGCCTCATGCGATTGCCGCAATT
AGTATGGCAAATTAA
>ctxB11
ATGATTAAATTAAAATTTGGTGTTTTTTTTACAGTTTTACTATCTTCAGCATATGCACAT
GGAACACCTCAAAATATTACTGATTTGTGTGCAGAATACCCCAACACACAAATACATACG
CTAAATGATAAGATATTTTCGTATACAGAATCTCTAGCTGGAAAAAGAGAGATGGCTATC
ATTACTTTTAAGAATGGTGCAACTTTTCAAGTAGAAGTACCAGGTAGTCAACATATAGAT
TCACAAAAAAAAGCGATTGAAAGGATGAAGGATACCCTGAGGATTGCATATCTTACTGAA
GCTAAAGTCGAAAAGTTATGTGTATGGAATAATAAAACGCCTCATGCGATTGCCGCAATT
AGTATGGCAAATTAA
>ctxB12
ATGATTAAATTAAAATTTGGTGTTTTTTTTACAGTTTTACTATCTTCAGCATATGCACAT
GGAACACCTCAAAATATTACTGATTTGTGTGCAGAATACCACAACGCACAAATATATACG
CTAAATGATAAGATATTGTCGTATACAGAATCTCTAGCTGGAAACAGAGAGATGGCTATC
ATTACTTTTAAGAATGGTGCAACTTTTCAAGTAGAAGTACCAGGTAGTCAACATATAGAT
TCACAAAAAAAAGCGATTGAAAGGATGAAGGATACCCTGAGGATTGCATATCTTACTGAA
GCTAAAGTCGAAAAGTTATGTGTATGGAATAATAAAACGCCTCATGCGATTGCCGCAATT
AGTATGGCAAATTAA
>ctxB2
ATGATTAAATTAAAATTTGGTGTTTTTTTTACAGTTTTACTATCTTCAGCATATGCACAT
GGAACACCTCAAAATATTACTGATTTGTGTGCAGAATACCACAACACACAAATACATACG
CTAAATGATAAGATATTGTCGTATACAGAATCTCTAGCTGGAAAAAGAGAGATGGCTATC
ATTACTTTTAAGAATGGTGCAACTTTTCAAGTAGAAGTACCAGGTAGTCAACATATAGAT
TCACAAAAAAAAGCGATTGAAAGGATGAAGGATACCCTGAGGATTGCATATCTTACTGAA
GCTAAAGTCGAAAAGTTATGTGTATGGAATAATAAAACGCCTCATGCGATTGCCGCAATT
AGTATGGCAAATTAA
```

If you want a more detailed history of the FASTA file format, then you could take a look at the Wikipedia page here: https://en.wikipedia.org/wiki/FASTA_format.

### FASTQ

The widely used FASTA file format has the great advantage of simplicity. However, this simplicity can be restrictive if we want to include additional data/metadata in addition to the sequence.

Given the non-negligible error rates of NGS technologies, often we need to accompany our sequence data with quality scores that estimate our confidence in the accuracy of the sequence data. As we will see later, this allows us to perform quality control checks and filter-out poor-quality data before performing analyses.

**FASTQ** is a simple text-based format that allows us to include quality scores. A single sequence is represented by four lines of text:

    @ERR8261968.1 1 length=97
    ACTTTCGATCTCTTGTAGATCTGTTCTCTAAACGAACTTTAAAATCTGTGTGGCTGTCACTCGGCTGCATGCTTAGTGCACTCACGCAGTATAATTA
    +ERR8261968.1 1 length=97
    CCCCCFDDFFFFGGGGGGGGGGHHHHHHHHHHHGGGGHHHHHHHHHHHHHHHGHHGHHIIHHGGGGGGHHHHHHHHHHHHHHHHHHHGGGHHHHHHH

- The first line is a 'header' containing a unique identifier for the sequence and, optionally, further description.
- The second line contains the actual nucleotide sequence.
- The third line is redundant  and can be safely ignored. Sometimes it simply repeats the first line. Sometimes it is blank or just contains a '+' character.
- The fourth line contains a string of characters that encode quality scores for each nucleotide in the sequence on [ASCII code](https://en.wikipedia.org/wiki/ASCII). 

Each single character encodes a score, typically a number between 0 and 40; this score is encoded by a single character.

| Character | ASCII | FASTQ quality score (ASCII – 33) 
| --|--|--
| ! | 33 | 0
| “ | 34 | 1
| # | 35 | 2
| $ | 36 | 3
| % | 37 | 4
| ... | ... | ...
| C | 67 | 34
| D | 68 | 35
| E | 69 | 36
| F | 70 | 37
| G | 71 | 38
| H | 72 | 39
|40 | 73 | 40

So, in the example above, we can see that most of the positions within the 97-nucleotide sequence have scores in the high 30s, which indicates a high degree of confidence in their accuracy.
- A score of 30 denotes a 1 in 1000 chance of an error, i.e. 99.9% accuracy.
- A score of 40 denotes a 1 in 10,000 chance of an error, i.e. 99.99% accuracy.

You can read more about the FastQ file format and quality scores here:

Cock, P. J., Fields, C. J., Goto, N., Heuer, M. L., & Rice, P. M. (2010). The Sanger FASTQ file format for sequences with quality scores, and the Solexa/Illumina FASTQ variants. *Nucleic Acids Research*, **38**, 1767–1771. https://doi.org/10.1093/nar/gkp1137.

### SAM and BAM

A **SAM file** (usually named *.sam) is used to represent aligned sequences. It is particularly useful for storing the results of genomic or transcriptomic sequence reads aligned against a reference genome sequence. The **BAM file** format is a compressed form of SAM. This has the disadvantage that it is not readable by a human but has the advantage of being smaller than the corresponding SAM file and thus easier to share and copy between locations.

You can read about SAM and BAM formats here:
 - Li, H., Handsaker, B., Wysoker, A., Fennell, T., Ruan, J., Homer, N., Marth, G., Abecasis, G., Durbin, R., & 1000 Genome Project Data Processing Subgroup (2009). The Sequence Alignment/Map format and SAMtools. *Bioinformatics*, **25**, 2078–2079. https://doi.org/10.1093/bioinformatics/btp352
-  [https://samtools.github.io/hts-specs/SAMv1.pdf](https://samtools.github.io/hts-specs/SAMv1.pdf).

We can view BAM files graphically using specialised genome browsers software such as (we will be working with one of them in the Genome Visualisation Tools module):
- [IGV](https://igv.org/)
- [Tablet](https://ics.hutton.ac.uk/tablet/)
- [Artemis / BAMview](http://sanger-pathogens.github.io/Artemis/Artemis/) 

An example **SAM file** is shown in the image below, along with the 11 mandatory fields of this standard:

![SAMformat](https://user-images.githubusercontent.com/65819144/230166077-264a6161-ec8c-47d7-87b7-8e073080a548.jpg)

(_Image from [Data Wrangling and Processing for Genomics](https://datacarpentry.org/wrangling-genomics)_)

The meaning of each of these mandatory fields are detailed below:

![SAMformat_fields](https://user-images.githubusercontent.com/65819144/230166180-fb086380-413a-4eef-8781-d0ba2b7e215b.jpg)

## [Quality control for FastQ data](#quality-control-for-FastQ-data)

Modern high throughput sequencers can generate hundreds of millions of sequences in a single run. Before analysing this sequence to draw biological conclusions you should always perform some simple quality control checks to ensure that the raw data looks good and there are no problems or biases in your data which may affect how you can usefully use it.

Most sequencers will generate a QC report as part of their analysis pipeline, but this is usually only focused on identifying problems which were generated by the sequencer itself. To check the quality of the reads generated, we will be using the software [**FastQC**](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) which will provide a QC report that can spot problems which originate either in the sequencer or in the starting library material. 

FastQC can be run in one of two modes. It can either run as a stand alone interactive application for the immediate analysis of small numbers of FastQ files, or it can be run in a non-interactive mode where it would be suitable for integrating into a larger analysis pipeline for the systematic processing of large numbers of files.

![fastqc](https://user-images.githubusercontent.com/65819144/230931477-4ed32628-11d5-4b1a-bfe6-70633185eeec.png)

FastQC will highlight any areas where the library looks unusual and where you should take a closer look. The program is not tied to any specific type of sequencing technique and can be used to look at libraries coming from a large number of different experiment types (Genomic Sequencing, ChIP-Seq, RNA-Seq, BS-Seq etc etc).

Different softwares for assesing the quality of reads have been developed. Another useful software for checking the quality of long-reads is **NanoPlot**. NanoPlot assess characteristics relevant to long-read genome sequencing and produces different number of plots depending on the data. A detailed view can be seen [here](https://github.com/wdecoster/NanoPlot#plots-generated).

It is very common to have some quality metrics fail after running FastQC, and this may or may not be a problem for your downstream application. But don't worry there are softwares developed to filter poor quality reads and trim poor quality bases or adapters from our samples. In this module we will be working with [**TrimGalore**](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) for short reads and with [**Filtlong**](https://github.com/rrwick/Filtlong) for Oxford Nanopore reads.

>**Note**: **Porechop** is a tool for finding and removing adapters from Oxford Nanopore reads. Adapters on the ends of reads are trimmed off, and when a read has an adapter in its middle, it is treated as chimeric and chopped into separate reads. We won't be using this software in this module but you could try it with your data! More information [here](https://github.com/rrwick/Porechop).

Last but not least, it is always a good idea to check that your data belongs to the species you expect it to be before you carry on with your downstream analysis. We could identify contamination using either one of two ways:
1) The GC content varies between species, so a shift in GC content could be an indication of sample contamination.
2) Using specialized tools to determine/predict the species composition of your sample. We will determine species composition using [**kraken2**](https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown). Kraken is a taxonomic sequence classifier that assigns taxonomic labels to DNA sequences. Kraken examines the k-mers within a query sequence and uses the information within those k-mers to query a database.

    **What are k-mers?**
    A k-mer is a sequence of k characters in a DNA sequence.
    So, for example, the sequence ATCGATCAC contains the following 3-mers (k-mer of size 3):
    ![k-mer](https://github.com/user-attachments/assets/e0e4cf3e-d055-42cc-8f5b-ec42d2236626)

# [Practical Exercise](#practical-exercise)

For this module, we will be working with files which are inside the folder "Module_1_Sequencing_QC". Do you remember how to change to this directory? Give it a try!

## [QC of short data](#QC-of-short-data)

Inside the directory "Module_1_Sequencing_QC", we have 7 compressed fastq files as fastq.gz. You can check that the files are there. Do you remember how to do that?

We will start working with files:
 - `CNGB1553_S31_L001_R1_001.fastq.gz`
 - `CNGB1553_S31_L001_R2_001.fastq.gz`


As we saw in the introduction, this is the format we will get from an Illumina paired-end run. To have a better look at their structure, we will uncompress them:
```
gzip -d CNGB1553_S31_L001_R1_001.fastq.gz
```
Wait until the command line is shown again on the screen and then type:
```
gzip -d CNGB1553_S31_L001_R2_001.fastq.gz
```

The files should be decompressed now. Let’s have a look at one of the fastq files:

```
head CNGB1553_S31_L001_R1_001.fastq
```

You should see something like this:

```
@M06899:234:000000000-L956R:1:1101:18268:1886 1:N:0:CGTACAGGAA+TAGGTTCTCT
TCACATCACCCTGCTGTGCAATCGCTCTCGCCAGAAAGACGCGTTTCTTTTGTCCACCTTAAAGCTCGCCGATTTTTCGCTGTCTCTTATACACATCTCCGAGCCCACGTGACCGTACAGGAAATCTCGTATGCCGTCTTCTGCTTGAATTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTT
+
A11>1DC1D>CAGFGCGGFEGGHGCGFHD0AAAE0AF10A//AEEE1FHHHBGAAA1B0111111B@/////>?F1B0//?>E@FDGH1F222>1B>B1>////<///////<1>?<AAG0000@1@<0?/?011/?->F<F11=F10111110-----:---9----9---9--------9-9---9-9------99--9-9-9---999----99--999-9-9--9--9-99---------99-9>--
@M06899:234:000000000-L956R:1:1101:18433:1899 1:N:0:CGTACAGGAA+TAGGTTCTCT
GTTCTGCAAAACACGAAAAACAAAGGTGACGTTGCGATCGGTGCAACGGTAACTGATGCCTCCGCGGTACTGGCTACAGATTACAAAATCTCGTTCGATAATAATCAGTGGCAGGTCACCCGCCTTGCCAGTAATACCACTTTTACGGTGACGCCGTATGCCAACTGTAAAGCTGTCTCTTATACACATCTCCGATCCCACGAGACCTTACAGGTAATTTCTTATGCCGTCTTCTTCTTTTAATAAATTTT
+
1AAAABDDFBCCFGE?11EAEGH00BGF1AAFFEE?/FA//E/E11A//B/ABG11F111D1/////E/>FB1/01>111BF1@1110B>GE0?F0//B//B>2FE11B20/0//FFFFFG//<?C11111?11@11111?GHH11<.>..<..<--.</0<00.//<00000;0<0C0GG0C00090;9B00.-.-.909---.99/9/9//////9/;//9/////-;-9;/9//9B////////////
```

**Remembering the fastq file format, how many reads do you have represented in the previous box?**

Now, let’s check how many reads we have in CNGB1553_S31_L001_R1_001.fastq and CNGB1553_S31_L001_R2_001.fastq, and double check we have the same number in the R1 and R2 files. We can use the `wc` command with the -l (letter elle) option to count the number of lines in the file:

```
wc -l CNGB1553_S31_L001_R1_001.fastq
```
**How many paired reads are in the sample?**
>Hint: Think about how many lines a single read takes up in FASTQ format.

**Does the number of reads in R1 and R2 files match?**

Let's examine the quality of all the sequence data present in the "Module_1_Sequencing_QC" folder using [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/).  

First, for the sake of simplicity, let’s compress again the two fastq files belonging to sample CNGB1553:

```
gzip CNGB1553_*.fastq
```

Going back to FastQC, we can launch the graphical interface by simply executing ``fastqc`` on the command line. However, it is often more convenient to use the software in the command-line mode. Execute the following commands in the Terminal:

1. `mkdir fastqc_raw`


2. `fastqc -o fastqc_raw *.fastq.gz`


> First run the `mkdir` command and afterwards `fastqc`.

> With this command we will be running FastQC in all the files that have the `.fastq.gz` extension and the output files will appear in the folder "fastqc_raw".

> **Note**: instead of compressing the files again with gzip, you could also use the command `fastqc *.fastq*`. By adding a star symbol after `.fastq`, fastqc will run in all files that include `.fastq` as part of the extension (zipped and unzipped)
         
Once FastQC has finished, execute the command

    ls -lh fastqc_raw

You should see some new files have appeared inside the directory "fastqc_raw".

We are most interested in the HTML files, which contain the FastQC reports for our fastq files. Let's open the HTML files generated for all the samples we are working with.

Use the following command as an example:

    firefox fastqc_raw/CNGB1553_*.html &

> **Note**: Don't worry if error messages appear on the command line, just hit enter to get back the prompt.

> You can open HTML files not only through the command line but also by navigating to the folder and double-clicking the file to open it in your preferred browser. 

You should then see something like this:

![Imagen1_FastQC](https://github.com/user-attachments/assets/25c170cd-fab5-4cea-9dee-9cccd77ee807)

There is a lot of QC information in these reports. Feel free to explore these in your own time and take a look at the [FastQC homepage](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) where you can find the [explanation](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/) to each report FastQC is generating and we recommend you to watch the tutorial video at http://www.youtube.com/watch?v=bz93ReOv87Y.

For now, we are just going to look at
- Basic statistics
- Per-base sequence quality
- Per sequence GC content 

Try to answer the following questions analysing FastQC results for samples CNGB1553, CNGB1797 and CNGB39120:

**Analysing the FastQC reports, how many reads are there? Does this answer match your previous answer for sample CNGB1553 (based on ``wc`` command)?**

**What is the average depth of coverage for these samples??**
> **Note**: Consider the following genome sizes: CNGB1553 (5 Mb), CNGB1797 (2.8 Mb) and CNGB39120 (4.4 Mb)

**With respect to quality scores, is the quality of read 1 exactly the same as that of read 2? Do you see the same in all the samples?**

Before we continue working with our data, we want to check that there are no contaminations with other organisms. Although you may have a clue after analysing the "Per sequence GC content" graphs, we will double check by using [kraken2](https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown). 

> **Note**: Before running kraken2 be sure to close firefox as it may not work if you have firefox open. Additionally, try to leave kraken2 working without doing anything else in the VM. It might take a while.

Execute the following command to run kraken2 on sample CNGB1553:

```
mkdir kraken2
```

```
conda activate kraken2
```

```
kraken2 -db /home/manager/kraken2_database --threads 2 --gzip-compressed --paired --report kraken2/CNGB1553_kraken2.txt --use-names CNGB1553_S31_L001_R1_001.fastq.gz CNGB1553_S31_L001_R2_001.fastq.gz
```
-db = name for Kraken2 database

--threads = number of threads

--gzip-compressed = input files are compressed with gzip

--paired = the filenames provided have paired-end reads

--report = print a report with aggregrate counts/clade to file

--use-names = print scientific names instead of just taxids

Repeat the previous command for samples CNGB1797 and CNGB39120.

When you finish running kraken2, please type:

```
conda deactivate
```

**What is the most abundant species identified in each sample?**

**Are samples contaminated with other bacterial species?**

Now, we are going to look at how we can remove poor data and adapter contamination by trimming and filtering. We will use [TrimGalore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) by executing the following commands on the Terminal:

```
mkdir trimmed
```

```
trim_galore -q 25 --length 50 --paired --illumina --fastqc -o trimmed CNGB1553_S31_L001_R1_001.fastq.gz CNGB1553_S31_L001_R2_001.fastq.gz
```

-q 25 = trim the 3’ end of the reads – remove nucleotides less than Phred Quality 25

--length 50 = after adapter and quality trimming, remove reads less than length 50bp

--paired = the names of the paired FASTQ files to analyses in order

--illumina = adapter sequence to be trimmed is the first 13bp of the Illumina universal adapter AGATCGGAAGAGC instead of the default auto-detection of adapter sequence

--fastqc = run FastQC in the default mode on the FastQ file once trimming is complete

-o = all output will be written to this directory

Once Trim Galore has finished, check the outputs inside the folder "trimmed". You should see that two new trimmed FASTQ (.fq) files have been created as well as FastQC reports:

CNGB1553_S31_L001_R1_001_val_1.fq.gz

CNGB1553_S31_L001_R2_001_val_2.fq.gz

**How many paired reads are left in the sample after trimming? Compare them with the fastq files obtained from the sequencer**

Now, do the same for samples CNGB1797 and CNGB39120 and compare the outputs with their respective untrimmed files.

## [QC of long data](#QC-of-long-data)

Let's check the quality of the reads for sample CTMA_1441 which was sequenced by an Oxford Nanopore sequencer:

```
NanoPlot -o nanoplot/CTMA_1441_longds --tsv_stats --info_in_report --N50 --no_static --prefix CTMA_1441_longds_ --fastq CTMA_1441_longds.fastq.gz
```

Let's take a look at the outputs generated by Nanoplot:

```
ls nanoplot/CTMA_1441_longds
```

You'll see different .html and .txt files with different plots and information which are summarised in the report: 

```
firefox nanoplot/CTMA_1441_longds/CTMA_1441_longds_NanoPlot-report.html &
```

**How many reads are there in CTMA_1441_longds.fastq.gz?**
**What is the median read length, N50 and median quality score?**

Before continuing, we will check there are no contaminations in this dataset:

```
conda activate kraken2
```

```
kraken2 -db /home/manager/kraken2_database --threads 4 --gzip-compressed --report kraken2/CTMA_1441_kraken2.txt --use-names CTMA_1441_longds.fastq.gz
```

```
conda deactivate
```

**What is the most abundant species identified in this sample?**

Now, let's improve the quality of our long reads using Filtlong. We will remove the worst reads (short length and low accuracy) with --keep_percent 90 and just keep those reads which are longer than 1000 kb (--min-length 1000):

```
filtlong --min_length 1000 --keep_percent 90 CTMA_1441_longds.fastq.gz | gzip > trimmed/CTMA_1441_longds_filt.fastq.gz
```

Adapt the previous code to run NanoPlot on the newly created long-read filtered file (CTMA_1441_longds_filt.fastq.gz)

> Remember the trimmed fastq file is inside the folder "trimmed". 

**Examine the results for the filtered file and try to answer the questions you answered for the unfiltered file. Do you find any difference among these results?**

## [Bonus](#bonus)

#### What if you had a large number of FastQC and kraken2 reports to analyse?

[**MultiQC**](https://multiqc.info/) is a tool that summarises different types of NGS reports. It is really interesting for visualising many FastQC and kraken2 reports at once.

Being inside Module_1_Sequencing_QC folder, run the following command:

    multiqc fastq_raw kraken2

See summarized report in a browser:

    firefox multiqc_report.html &

[**Pavian**](https://github.com/fbreitwieser/pavian) is another interactive browser application for analysing and visualisation of results from classifiers such as Kraken2.

First, navigate to [https://fbreitwieser.shinyapps.io/pavian/](https://fbreitwieser.shinyapps.io/pavian/).

Click on "Browse" and upload the 4 kraken reports we´ve obtained during this module.

Once the upload is complete, click on the "Comparison" tab located on the left-hand side of the screen.

You can decide how to visualise the results clicking on the different tabs "Reads", "%", "Any Rank", "Domain", etc. Additionally, by clicking on a specific sample, the visualisation will update to reflect the taxonomic composition of that sample.


## [Wrap up questions](#Wrap-up-questions)

1) What information did you get about the sequencing runs?
> Read length? Probable platform?
2) What do you think about the quality of the sequenced data analysed in this module? 
3) Did the trimming solve all the quality issues encountered in all the samples?
4) Would you use these sequences for downstream analysis? Why?



[<<< Go back to Manual Contents Page](https://github.com/WCSCourses/GenEpiLAC2025/blob/main/course_modules_2025/Manual_main.md)

<br>
