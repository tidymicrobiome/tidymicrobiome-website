---
date: "2019-05-05T00:00:00+01:00"
title: "Merge data"
linktitle: "4 Merge data"
author: admin
type: book
weight: 4
commentable: true
---





Merging OTU or sample indices based on variables in the data can be a useful means of reducing noise or excess features in an analysis or graphic. 

## Loading included data


```r
library(microbiomedataset)
library(tidyverse)
data("global_patterns")
global_patterns
#> -------------------- 
#> microbiomedataset version: 0.99.1 
#> -------------------- 
#> 1.expression_data:[ 19216 x 26 data.frame]
#> 2.sample_info:[ 26 x 8 data.frame]
#> 3.variable_info:[ 19216 x 8 data.frame]
#> 4.sample_info_note:[ 8 x 2 data.frame]
#> 5.variable_info_note:[ 8 x 2 data.frame]
#> -------------------- 
#> Processing information (extract_process_info())
#> create_microbiome_dataset ---------- 
#>             Package               Function.used                Time
#> 1 microbiomedataset create_microbiome_dataset() 2022-07-10 10:56:13
```

## Merge samples

Remove empty taxa


```r
global_patterns2 <-
  global_patterns %>%
  mutate2variable(what = "sum_intensity") %>%
  activate_microbiome_dataset(what = "variable_info") %>%
  dplyr::filter(sum_intensity > 0)

humantypes <- c("Feces", "Mock", "Skin", "Tongue")
global_patterns2 <-
  global_patterns2 %>%
  activate_microbiome_dataset(what = "sample_info") %>%
  dplyr::mutate(human = SampleType %in% humantypes)
```

Now on to the merging examples.


```r
merged_global_patterns2 <- 
  microbiomedataset::summarise_samples(object = global_patterns2, 
                                       group_by = "SampleType")
extract_sample_info(merged_global_patterns2)
#>            sample_id  Primer Final_Barcode Barcode_truncated_plus_T
#> 1               Soil ILBC_01        AACGCA                   TGCGTT
#> 2              Feces ILBC_04        AAGAGA                   TCTCTT
#> 3               Skin ILBC_07        AATCGT                   ACGATT
#> 4             Tongue ILBC_10        ACACGA                   TCGTGT
#> 5         Freshwater ILBC_13        ACACTG                   CAGTGT
#> 6 Freshwater (creek) ILBC_16        ACAGCA                   TGCTGT
#> 7              Ocean ILBC_19        ACAGTT                   AACTGT
#> 8 Sediment (estuary) ILBC_22        ACATGT                   ACATGT
#> 9               Mock ILBC_27        ACCGCA                   TGCGGT
#>   Barcode_full_length         SampleType
#> 1         CTAGCGTGCGT               Soil
#> 2         TCGACATCTCT              Feces
#> 3         CGAGTCACGAT               Skin
#> 4         TGTGGCTCGTG             Tongue
#> 5         CATGAACAGTG         Freshwater
#> 6         GACCACTGCTG Freshwater (creek)
#> 7         TCGCGCAACTG              Ocean
#> 8         CACGTGACATG Sediment (estuary)
#> 9         TGACTCTGCGG               Mock
#>                                    Description   class human
#> 1     Calhoun South Carolina Pine soil, pH 4.9 Subject FALSE
#> 2      M3, Day 1, fecal swab, whole body study Subject  TRUE
#> 3      M3, Day 1, right palm, whole body study Subject  TRUE
#> 4         M3, Day 1, tongue, whole body study  Subject  TRUE
#> 5 Lake Mendota Minnesota, 24 meter epilimnion  Subject FALSE
#> 6                 Allequash Creek, 0-1cm depth Subject FALSE
#> 7       Newport Pier, CA surface water, Time 1 Subject FALSE
#> 8               Tijuana River Reserve, depth 1 Subject FALSE
#> 9                                        Even1 Subject  TRUE
```

## Merge taxas


```r
merged_variables <-
  microbiomedataset::summarize_variables(
    object = global_patterns2,
    variable_index = 1:5,
    remain_variable_info_index = 1
  )
dim(merged_variables)
#> variables   samples 
#>     18984        26
dim(global_patterns2)
#> variables   samples 
#>     18988        26
```


## Session information


```r
sessionInfo()
#> R version 4.3.0 (2023-04-21)
#> Platform: x86_64-apple-darwin20 (64-bit)
#> Running under: macOS Ventura 13.5.2
#> 
#> Matrix products: default
#> BLAS:   /Library/Frameworks/R.framework/Versions/4.3-x86_64/Resources/lib/libRblas.0.dylib 
#> LAPACK: /Library/Frameworks/R.framework/Versions/4.3-x86_64/Resources/lib/libRlapack.dylib;  LAPACK version 3.11.0
#> 
#> locale:
#> [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
#> 
#> time zone: America/Los_Angeles
#> tzcode source: internal
#> 
#> attached base packages:
#> [1] stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] lubridate_1.9.2           forcats_1.0.0            
#>  [3] stringr_1.5.0             dplyr_1.1.2              
#>  [5] purrr_1.0.1               readr_2.1.4              
#>  [7] tidyr_1.3.0               tibble_3.2.1             
#>  [9] ggplot2_3.4.2             tidyverse_2.0.0          
#> [11] microbiomedataset_0.99.10
#> 
#> loaded via a namespace (and not attached):
#>   [1] RColorBrewer_1.1-3          rstudioapi_0.14            
#>   [3] jsonlite_1.8.5              shape_1.4.6                
#>   [5] magrittr_2.0.3              farver_2.1.1               
#>   [7] MALDIquant_1.22.1           rmarkdown_2.22             
#>   [9] GlobalOptions_0.1.2         zlibbioc_1.46.0            
#>  [11] vctrs_0.6.2                 multtest_2.56.0            
#>  [13] RCurl_1.98-1.12             blogdown_1.18.1            
#>  [15] htmltools_0.5.5             S4Arrays_1.0.4             
#>  [17] Rhdf5lib_1.22.0             rhdf5_2.44.0               
#>  [19] gridGraphics_0.5-1          mzID_1.38.0                
#>  [21] sass_0.4.6                  bslib_0.5.0                
#>  [23] htmlwidgets_1.6.2           plyr_1.8.8                 
#>  [25] zoo_1.8-12                  plotly_4.10.2              
#>  [27] impute_1.74.1               cachem_1.0.8               
#>  [29] igraph_1.4.3                lifecycle_1.0.3            
#>  [31] iterators_1.0.14            pkgconfig_2.0.3            
#>  [33] Matrix_1.5-4                R6_2.5.1                   
#>  [35] fastmap_1.1.1               GenomeInfoDbData_1.2.10    
#>  [37] MatrixGenerics_1.12.2       clue_0.3-64                
#>  [39] digest_0.6.31               pcaMethods_1.92.0          
#>  [41] colorspace_2.1-0            masstools_1.0.10           
#>  [43] S4Vectors_0.38.1            rprojroot_2.0.3            
#>  [45] GenomicRanges_1.52.0        vegan_2.6-4                
#>  [47] timechange_0.2.0            fansi_1.0.4                
#>  [49] httr_1.4.6                  mgcv_1.8-42                
#>  [51] polyclip_1.10-4             compiler_4.3.0             
#>  [53] here_1.0.1                  remotes_2.4.2              
#>  [55] withr_2.5.0                 doParallel_1.0.17          
#>  [57] BiocParallel_1.34.2         viridis_0.6.3              
#>  [59] ggforce_0.4.1               MASS_7.3-58.4              
#>  [61] DelayedArray_0.26.3         biomformat_1.28.0          
#>  [63] rjson_0.2.21                permute_0.9-7              
#>  [65] ggsci_3.0.0                 mzR_2.34.0                 
#>  [67] tools_4.3.0                 ape_5.7-1                  
#>  [69] zip_2.3.0                   glue_1.6.2                 
#>  [71] rhdf5filters_1.12.1         nlme_3.1-162               
#>  [73] grid_4.3.0                  cluster_2.1.4              
#>  [75] reshape2_1.4.4              ade4_1.7-22                
#>  [77] generics_0.1.3              gtable_0.3.3               
#>  [79] tzdb_0.4.0                  preprocessCore_1.62.1      
#>  [81] data.table_1.14.8           hms_1.1.3                  
#>  [83] tidygraph_1.2.3             utf8_1.2.3                 
#>  [85] XVector_0.40.0              BiocGenerics_0.46.0        
#>  [87] ggrepel_0.9.3               foreach_1.5.2              
#>  [89] pillar_1.9.0                yulab.utils_0.0.6          
#>  [91] limma_3.56.2                splines_4.3.0              
#>  [93] circlize_0.4.15             tweenr_2.0.2               
#>  [95] lattice_0.21-8              survival_3.5-5             
#>  [97] tidyselect_1.2.0            ComplexHeatmap_2.16.0      
#>  [99] Biostrings_2.68.1           pbapply_1.7-0              
#> [101] knitr_1.43                  gridExtra_2.3              
#> [103] bookdown_0.34               phyloseq_1.44.0            
#> [105] IRanges_2.34.0              ProtGenerics_1.32.0        
#> [107] SummarizedExperiment_1.30.2 Rdisop_1.60.0              
#> [109] stats4_4.3.0                xfun_0.39                  
#> [111] graphlayouts_1.0.0          Biobase_2.60.0             
#> [113] MSnbase_2.26.0              matrixStats_1.0.0          
#> [115] stringi_1.7.12              lazyeval_0.2.2             
#> [117] yaml_2.3.7                  evaluate_0.21              
#> [119] codetools_0.2-19            ggraph_2.1.0               
#> [121] MsCoreUtils_1.12.0          BiocManager_1.30.21        
#> [123] ggplotify_0.1.0             cli_3.6.1                  
#> [125] affyio_1.70.0               munsell_0.5.0              
#> [127] jquerylib_0.1.4             Rcpp_1.0.10                
#> [129] GenomeInfoDb_1.36.0         png_0.1-8                  
#> [131] XML_3.99-0.14               parallel_4.3.0             
#> [133] bitops_1.0-7                tidytree_0.4.2             
#> [135] viridisLite_0.4.2           scales_1.2.1               
#> [137] affy_1.78.0                 openxlsx_4.2.5.2           
#> [139] ncdf4_1.21                  crayon_1.5.2               
#> [141] GetoptLong_1.0.5            rlang_1.1.1                
#> [143] massdataset_1.0.25          vsn_3.68.0
```
