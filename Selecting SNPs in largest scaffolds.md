
**1. This command gets the name of scaffolds in my reference genome and sort them by length.**
  * `cut` (selects the first and second columns of `fa.fai`
  * `sort` sorts by the second column
  * `tac` reverses the order

`cut -f1,2 Picoides_pubescens_ref.fa.fai | sort -k2,2n | tac > Picoides_pubescens_sizes.genome`

**2. This command selects only scaffolds above 50 kbp.**

`awk -F"\t" '$2>50000' Picoides_pubescens_sizes.genome > Scaffolds_above_50k`

**3. This command sorts the vcf file by scaffold, according to the order of the `Picoides_pubescens_sizes.genome` file.**

`bedtools sort -faidx Picoides_pubescens_sizes.genome -i SNP-only.vcf -header > SNP-only_sorted.vcf`

**4. These commands compresses and indexes the vcf file.**

`bgzip SNP-only_sorted.vcf`

`tabix -p vcf SNP-only_sorted.vcf.gz`

**5. This command selects only SNPs in the largest scaffolds.**

`bcftools filter -R Scaffolds_above_50k.txt SNP-only_sorted.vcf.gz -o SNP-only_above_50k.vcf`
