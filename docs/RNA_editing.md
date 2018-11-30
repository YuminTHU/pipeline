## long RNA-seq
### RNAEditor
Paper: RNAEditor: easy detection of RNA editing events and
the introduction of editing islands

Pipeline: http://rnaeditor.uni-frankfurt.de/index.php
```
RNAEditor -i Fastq-Files [Fastq-Files ...] -c Configuration File
```
## small RNA-seq
Paper: Identifying RNA Editing Sites in miRNAs by Deep Sequencing

Pipeline: https://www.tau.ac.il/~elieis/miR_editing/

###step1. Filtering Low- Quality Reads and Trimming Sequence Adapters
```
perl Process_reads.pl Input_fastq_file The_filtered_fastq_file
```
###step2. Aligning the reads against the genome
```
bowtie -n 1 -e 50 -a -m 1 --best --strata --trim3 2 The_bowtie_folder/The_genome_indexes The_filtered_fastq_file > The_output_file
```
###step3. Mapping the mismatches against the pre-miRNA sequences
```
perl Analyze_mutation.pl The_output_file main_output.txt
```
###step4. Using binomial statistics to remove sequencing errors
```
perl Binomial_analysis.pl main_output.txt >binomial_output.txt
```
