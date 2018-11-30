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


### DaPars
#### Paper
Dynamic analyses of alternative polyadenylation
from RNA-seq reveal a 3'UTR landscape across
seven tumour types
#### Website
#### Pipeline
