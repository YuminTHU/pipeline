## step1. get miRNA-mRNA interactions
### 1. download from database
miRWalk2.0: a comprehensive atlas of predicted and validated miRNA-target interactions.
http://zmf.umm.uni-heidelberg.de/apps/zmf/mirwalk2/

### 2. prediction by bioinformatics tools
miRanda
```
miranda miRNA.fa target_sequence.fa -strict >strict.output
```
other recommended tools: 

RNAhybrid: https://bibiserv2.cebitec.uni-bielefeld.de/rnahybrid

TargetScan: http://www.targetscan.org/vert_72/

specific for plant:

psRNATarget: http://plantgrn.noble.org/psRNATarget/

psRobot: http://omicslab.genetics.ac.cn/psRobot/

## step2. functional annoation of miRNA targets
https://github.com/dongzhuoer/diff_exp_2018_zhuoer/blob/master/DE-miRNA-pipeline.md
