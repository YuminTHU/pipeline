## long RNA-seq differential expression

### step1. pre-process and qc

We received cleaned reads without adaptors and low quality reads

```
fastqc {input.fastq}
```

### step2. clean rRNA reads 

```
bowtie -v 1 -M 1 -m 10000 --best --un {sample.norRNA.fq} -t -p 6 {rRNA_index} -1 {sample_R1.fq} -2 {sample_R2.fq} {sample.alnrRNA.txt}
```

### step3. map to genome 
```
STAR --genomeDir {genome_index} --readFilesIn {sample.norRNA_1.fq} {sample.norRNA_2.fq} --runThreadN 10 --outFileNamePrefix {output_dir} --outSAMtype BAM SortedByCoordinate
```

### step4. generate count matrix 
```
featureCounts -T 6 -s 2 -p -t exon -g gene_id -a {annotation.gtf} -o {sample.featurecounts.txt} {sample.bam}
```

### step6. generate rpkm matrix

edgeR: rpkm() 
```r
rpkm <- rpkm(countData,gene.lengths)
```

step7.differential expression 

edgeR https://lulab.gitbook.io/training/part-ii.-basic-bioinfo-analyses/3.differential-expression

## short RNA-seq 

step1. pre-process and qc 

the same as total RNA

step2. map to genome 

bowtie2 -x {bowtie2_genome_index} -q {sample.clean.fastq}|samtools view -bS > {sample.clean.bam}

step3. generate count matrix 

featureCounts -s 1 -t miRNA -g Name -M --fraction -T 6 -a {miRNA.gff3} -o {sample.featurecounts.txt} {sample.clean.bam}

step4. generate rpkm matrix 

the same as total RNA

step5. differential expression 

the same as total RNA
