## APAtrap
### Paper
APAtrap: identification and quantification
of alternative polyadenylation sites from
RNA-seq data

### Website
https://sourceforge.net/projects/apatrap/

### Pipeline
#### step1. identifyDistal3UTR

Refine annotated 3'UTRs and identify novel 3'UTRs or 3'UTR extensions.

For genome having long 3'UTR
```
identifyDistal3UTR -i Sample1.bedgraph Sample2.bedgraph -m hg19.genemodel.bed -o novel.utr.bed
```
For genome having short 3'UTR
```
identifyDistal3UTR -i Sample1.bedgraph Sample2.bedgraph -m rice.genemodel.bed -o novel.utr.bed -w 50 -e 5000
```

#### step2. predictAPA

Infer all potential APA sites and estimate their corresponding usages.

For genome having long 3'UTR
```
predictAPA -i Sample1.bedgraph Sample2.bedgraph -g 2 -n 1 1 -u hg19.utr.bed -o output.txt
```
For genome having short 3'UTR
```
predictAPA -i Sample1.bedgraph Sample2.bedgraph -g 2 -n 1 1 -u rice.utr.bed -o output.txt -a 50
```

#### step3. deAPA

Detect genes having significant changes in APA site usage between conditions.

```
deAPA(input_file, output_file, group1, group2, least_qualified_num_in_group1, least_qualified_num_in_group2, coverage_cutoff)
```


## DaPars
### Paper
Dynamic analyses of alternative polyadenylation
from RNA-seq reveal a 3'UTR landscape across
seven tumour types

### Website
http://lilab.research.bcm.edu/dldcc-web/lilab/zheng/DaPars_Documentation/html/DaPars.html

### Pipeline
#### step1. Generate region annotation
DaPars will use the extracted distal polyadenylation sites to infer the proximal polyadenylation sites based on the alignment wiggle files of two samples. The output in this step will be used by the next step.

```
python DaPars_Extract_Anno.py -b gene.bed -s symbol_map.txt -o extracted_3UTR.bed
```

#### step2. Prepare the configure file
The configure file is the only parameter for DaPars_main.py, which stores all the parameters.
The format of the configure is:
```
#The following file is the result of step 1.

Annotated_3UTR=hg19_refseq_extracted_3UTR.bed

#A comma-separated list of BedGraph files of samples from condition 1

Group1_Tophat_aligned_Wig=Condition_A_chrX.wig
#Group1_Tophat_aligned_Wig=Condition_A_chrX_r1.wig,Condition_A_chrX_r2.wig if multiple files in one group

#A comma-separated list of BedGraph files of samples from condition 2

Group2_Tophat_aligned_Wig=Condition_B_chrX.wig

Output_directory=DaPars_Test_data/

Output_result_file=DaPars_Test_data

#At least how many samples passing the coverage threshold in two conditions
Num_least_in_group1=1

Num_least_in_group2=1

Coverage_cutoff=30

#Cutoff for FDR of P-values from Fisher exact test.

FDR_cutoff=0.05


PDUI_cutoff=0.5

Fold_change_cutoff=0.59
```


#### step3. Main function to get final result
Run this function to get the final result. 
```
python DaPars_main.py configure_file
```


