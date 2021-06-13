# Single-copy-Orthogenes-with-missing-species

OrthoFinder detects single copy orthogroups using sequence similarity searches, but to get the single copy orthogroups with missing species, the below strategy can be used:

After executing OrthoFinder, many directories are created. Among which the files in **Orthogroup directory** can be used for the extraction of single copy orthogroups with missing species
**Orthogroups directory** -----> ***Orthogroups.GeneCount.tsv***, Orthogroups.tsv, Orthogroups.txt, Orthogroups_SingleCopyOrthologues.txt, Orthogroups_UnassignedGenes.tsv. 
***Orthogroups.GeneCount.tsv*** can be used to extract the orthogroups with missing species 
Each column in ***Orthogroups.GeneCount.tsv** contains the count of genes present in the orthogroup with respect to the species. The 27th column of the file contains the total count of genes from a particular orthogroup.
```
awk '{sum=0; for(i=2; i<NF; i++) if($i==1) sum += $i; if(sum==24) print $0}' Orthogroups. GeneCount.tsv | awk '{if($27==24) print$0}' > gene_count24_orthogroups.tsv
```
The above command finds the summation of each row and prints only the rows with sum equal to the number of species needed and ensures that the 27th column is also equal to the number of species, because the starting awk command does not break when it finds more than one gene count.
Now the ###gene_count24_orthogroups.tsv### contains all the orthogroups with some species missing in it and the first column contains the orthogroups names, so we can use the first column to extract the orthogroups from Orthogroup_Sequenes directory which contains all the orthogroups sequences files. 
```cut -f1 gene_count24_orthogroups.tsv > genecount24.txt
i=$(cat genecount24.txt| xargs)
for j in $i; do cp ../Orthogroup_Sequences/$(ls ../Orthogroup_Sequences/ | grep $j) ./gene_count24_fastafiles/; done;
```
