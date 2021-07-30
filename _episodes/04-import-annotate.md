---
# Please do not edit this file directly; it is auto generated.
# Instead, please edit 04-import-annotate.md in _episodes_rmd/
source: Rmd
title: "Importing and annotating quantified data into R"
teaching: XX
exercises: XX
questions:
- "How do we get our data into R?"
objectives:
- "Learn how to import the quantifications into a SummarizedExperiment object."
- "Learn how to add additional gene annotations to the object."
keypoints:
- "Key point 1"
---





# Read the data

## Counts


~~~
counts <- read.csv("data/GSE96870_counts_cerebellum.csv", 
                   row.names = 1)
~~~
{: .language-r}

## Sample annotations


~~~
coldata <- read.csv("data/GSE96870_coldata_cerebellum.csv",
                    row.names = 1)
~~~
{: .language-r}

## Gene annotations

Need to be careful - the descriptions contain both commas and ' (e.g., 5')


~~~
rowdata <- read.delim("data/GSE96870_rowdata.tsv", sep = "\t", 
                      header = TRUE, quote = "",
                      row.names = 1)
~~~
{: .language-r}



~~~
Warning in file(file, "rt"): cannot open file 'data/GSE96870_rowdata.tsv': No
such file or directory
~~~
{: .warning}



~~~
Error in file(file, "rt"): cannot open the connection
~~~
{: .error}

Mention other ways of getting annotations, and practice querying org package.
Important to use the right annotation source/version. 


~~~
suppressPackageStartupMessages({
    library(org.Mm.eg.db)
})
mapIds(org.Mm.eg.db, keys = "497097", column = "SYMBOL", keytype = "ENTREZID")
~~~
{: .language-r}



~~~
'select()' returned 1:1 mapping between keys and columns
~~~
{: .output}



~~~
497097 
"Xkr4" 
~~~
{: .output}

Check feature types


~~~
table(rowdata$gbkey)
~~~
{: .language-r}



~~~
Error in eval(quote(list(...)), env): object 'rowdata' not found
~~~
{: .error}


# Assemble SummarizedExperiment


~~~
stopifnot(rownames(rowdata) == rownames(counts),
          rownames(coldata) == colnames(counts))
~~~
{: .language-r}



~~~
Error in h(simpleError(msg, call)): error in evaluating the argument 'x' in selecting a method for function 'rownames': object 'rowdata' not found
~~~
{: .error}



~~~
se <- SummarizedExperiment(
    assays = list(counts = as.matrix(counts)),
    rowData = rowdata,
    colData = coldata
)
~~~
{: .language-r}



~~~
Error in SummarizedExperiment(assays = list(counts = as.matrix(counts)), : object 'rowdata' not found
~~~
{: .error}

# Save SummarizedExperiment


~~~
saveRDS(se, "data/GSE96870_se.rds")
~~~
{: .language-r}



~~~
Error in saveRDS(se, "data/GSE96870_se.rds"): object 'se' not found
~~~
{: .error}

# Session info


~~~
sessionInfo()
~~~
{: .language-r}



~~~
R version 4.1.0 (2021-05-18)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Ubuntu 20.04.2 LTS

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.9.0
LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.9.0

locale:
 [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
 [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
 [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
[10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   

attached base packages:
[1] parallel  stats4    stats     graphics  grDevices utils     datasets 
[8] methods   base     

other attached packages:
 [1] org.Mm.eg.db_3.13.0         AnnotationDbi_1.54.1       
 [3] knitr_1.33                  SummarizedExperiment_1.22.0
 [5] Biobase_2.52.0              MatrixGenerics_1.4.0       
 [7] matrixStats_0.60.0          GenomicRanges_1.44.0       
 [9] GenomeInfoDb_1.28.1         IRanges_2.26.0             
[11] S4Vectors_0.30.0            BiocGenerics_0.38.0        

loaded via a namespace (and not attached):
 [1] Rcpp_1.0.7             compiler_4.1.0         XVector_0.32.0        
 [4] bitops_1.0-7           tools_4.1.0            zlibbioc_1.38.0       
 [7] bit_4.0.4              evaluate_0.14          RSQLite_2.2.7         
[10] memoise_2.0.0          lattice_0.20-44        pkgconfig_2.0.3       
[13] png_0.1-7              rlang_0.4.11           Matrix_1.3-3          
[16] DelayedArray_0.18.0    DBI_1.1.1              xfun_0.24             
[19] fastmap_1.1.0          GenomeInfoDbData_1.2.6 stringr_1.4.0         
[22] httr_1.4.2             Biostrings_2.60.1      vctrs_0.3.8           
[25] bit64_4.0.5            grid_4.1.0             R6_2.5.0              
[28] blob_1.2.2             magrittr_2.0.1         KEGGREST_1.32.0       
[31] stringi_1.7.3          RCurl_1.98-1.3         cachem_1.0.5          
[34] crayon_1.4.1          
~~~
{: .output}