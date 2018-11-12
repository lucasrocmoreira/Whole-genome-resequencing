**To create an index for reference genome to run Stampy**

* Make sure stampy is running in a python 2.7 environment 

```
conda -n stampy python=2.7
source activate stampy
```

1) First you have to build a genome file:

`stampy -G Picoides_pubescens Picoides_pubescens_ref-sorted.fa`

2) After building the genome file, you need to build a hash table:

`stampy -g Picoides_pubescens -H Picoides_pubescens`

3) Create a BWA index for the same reference genome as used by Stampy, then map the reads using BWA in the usual way, and use samtools to convert the resulting SAM file into a BAM file. Then re-map the BAM file using Stampy, but keep the well-mapped reads:

`stampy -g Picoides_pubescens -h Picoides_pubescens --substitutionrate 0.02 --bamkeepgoodreads -M bwa.bam -o stampy.sam -t 64`
