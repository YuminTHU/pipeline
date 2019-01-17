## Ribo-seq analysis

### step1. pre-processing
remove adapters
trim long reads
remove rRNA
```
STAR --genomeDir {rRNA_index} --runThreadN 10 --readFilesIn {input.fastq} --outFileNamePrefix {output_dir} -- outFilterMismatchNmax 1 --outSAMtype BAM Unsorted --outReadsUnmapped Fastx
```

### step2. map to genome
```
STAR --genomeDir {genome_index} --runThreadN 10 --readFilesIn {input.fastq} --outFileNamePrefix {output_dir} --alignEndsType EndToEnd --outFilterMismatchNmax 3 --outFilterMultimapNmax 8 --chimScoreSeparation 10 --chimScoreMin 20 --chimSegmentMin 15 --outSAMattributes All --outFilterIntronMotifs RemoveNoncanonicalUnannotated --alignSJoverhangMin 500 --outSAMtype BAM SortedByCoordinate
```
remove secondary alignment
```
samtools view -h {input_bam}|awk -F "\t" '$1~/@/ || $2<256' >{output.sam}
samtools view -b -S {output.sam} -o {output.bam}
samtools sort -m 5000000000 {output.bam} -o {output.sort.bam}
```
### step3. identify translated ORF
https://github.com/lulab/Ribowave
