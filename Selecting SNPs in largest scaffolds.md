
**1. This command gets the name of scaffolds in my reference genome and sort them by length.**
  * `cut` (selects the first and second columns of `fa.fai`
  * `sort` sorts by the second column
  * `tac` reverses the order

`cut -f1,2 Picoides_pubescens_ref.fa.fai | sort -k2,2n | tac > Picoides_pubescens_sizes.genome`

**2. This command selects only scaffolds above 50 kbp.**

`awk -F"\t" '$2>50000' Picoides_pubescens_sizes.genome > Scaffolds_above_50k`

`bedtools sort -faidx Picoides_pubescens_sizes.genome -i SNP-only.vcf -header > SNP-only_sorted.vcf`

`bgzip SNP-only_sorted.vcf`

`tabix -p vcf SNP-only_sorted.vcf.gz`

`bcftools filter -R Scaffolds_above_50k.txt SNP-only_sorted.vcf.gz -o SNP-only_above_50k.vcf`
