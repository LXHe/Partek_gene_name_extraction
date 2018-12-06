# Partek_gene_name_extraction

Many probes on Illumina MethylationEPIC BeadChip are annotated to several multiple gene regions of several genes. 
As presented in the file of 'demo_gene_extraction.csv', some reports will put these gene names and gene regions under one column,
which makes it difficult for further gene name extraction.

With the use of CountVectorizer from sklearn, the codes in this file aims to screen out duplicate information of gene names and 
regions in each row and recreate a summary file. 
