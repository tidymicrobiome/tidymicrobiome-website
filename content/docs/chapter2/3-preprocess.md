---
date: "2019-05-05T00:00:00+01:00"
title: "Functions for accessing and preprocessing data"
linktitle: "3 Functions for accessing and preprocessing data"
author: admin
type: book
weight: 3
commentable: true
---





## Loading included data

Let's load the demo data.


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

## Accessors


```r
dim(global_patterns)
#> variables   samples 
#>     19216        26
nrow(global_patterns)
#> variables 
#>     19216
ncol(global_patterns)
#> samples 
#>      26
```


```r
colnames(global_patterns)
#>  [1] "CL3"      "CC1"      "SV1"      "M31Fcsw"  "M11Fcsw"  "M31Plmr" 
#>  [7] "M11Plmr"  "F21Plmr"  "M31Tong"  "M11Tong"  "LMEpi24M" "SLEpi20M"
#> [13] "AQC1cm"   "AQC4cm"   "AQC7cm"   "NP2"      "NP3"      "NP5"     
#> [19] "TRRsed1"  "TRRsed2"  "TRRsed3"  "TS28"     "TS29"     "Even1"   
#> [25] "Even2"    "Even3"
```


```r
head(rownames(global_patterns))
#> [1] "549322" "522457" "951"    "244423" "586076" "246140"
```


```r
extract_sample_info(global_patterns) %>% 
  colnames()
#> [1] "sample_id"                "Primer"                  
#> [3] "Final_Barcode"            "Barcode_truncated_plus_T"
#> [5] "Barcode_full_length"      "SampleType"              
#> [7] "Description"              "class"
```


```r
extract_variable_info(global_patterns) %>% 
  colnames()
#> [1] "variable_id" "Kingdom"     "Phylum"      "Class"       "Order"      
#> [6] "Family"      "Genus"       "Species"
```



```r
extract_expression_data(global_patterns) %>% 
  head()
#>        CL3 CC1 SV1 M31Fcsw M11Fcsw M31Plmr M11Plmr F21Plmr M31Tong M11Tong
#> 549322   0   0   0       0       0       0       0       0       0       0
#> 522457   0   0   0       0       0       0       0       0       0       0
#> 951      0   0   0       0       0       0       1       0       0       0
#> 244423   0   0   0       0       0       0       0       0       0       0
#> 586076   0   0   0       0       0       0       0       0       0       0
#> 246140   0   0   0       0       0       0       0       0       0       0
#>        LMEpi24M SLEpi20M AQC1cm AQC4cm AQC7cm NP2 NP3 NP5 TRRsed1 TRRsed2
#> 549322        0        1     27    100    130   1   0   0       0       0
#> 522457        0        0      0      2      6   0   0   0       0       0
#> 951           0        0      0      0      0   0   0   0       0       0
#> 244423        0        0      0     22     29   0   0   0       0       0
#> 586076        0        0      0      2      1   0   0   0       0       0
#> 246140        0        0      0      1      3   0   0   0       0       0
#>        TRRsed3 TS28 TS29 Even1 Even2 Even3
#> 549322       0    0    0     0     0     0
#> 522457       0    0    0     0     0     0
#> 951          0    0    0     0     0     0
#> 244423       0    0    0     0     0     0
#> 586076       0    0    0     0     0     0
#> 246140       0    0    0     0     0     0
```


```r
extract_sample_info(global_patterns) %>% 
  head()
#>   sample_id  Primer Final_Barcode Barcode_truncated_plus_T Barcode_full_length
#> 1       CL3 ILBC_01        AACGCA                   TGCGTT         CTAGCGTGCGT
#> 2       CC1 ILBC_02        AACTCG                   CGAGTT         CATCGACGAGT
#> 3       SV1 ILBC_03        AACTGT                   ACAGTT         GTACGCACAGT
#> 4   M31Fcsw ILBC_04        AAGAGA                   TCTCTT         TCGACATCTCT
#> 5   M11Fcsw ILBC_05        AAGCTG                   CAGCTT         CGACTGCAGCT
#> 6   M31Plmr ILBC_07        AATCGT                   ACGATT         CGAGTCACGAT
#>   SampleType                                Description   class
#> 1       Soil   Calhoun South Carolina Pine soil, pH 4.9 Subject
#> 2       Soil   Cedar Creek Minnesota, grassland, pH 6.1 Subject
#> 3       Soil Sevilleta new Mexico, desert scrub, pH 8.3 Subject
#> 4      Feces    M3, Day 1, fecal swab, whole body study Subject
#> 5      Feces   M1, Day 1, fecal swab, whole body study  Subject
#> 6       Skin    M3, Day 1, right palm, whole body study Subject
```


```r
extract_variable_info(global_patterns) %>% 
  head()
#>   variable_id Kingdom        Phylum        Class        Order        Family
#> 1      549322 Archaea Crenarchaeota Thermoprotei         <NA>          <NA>
#> 2      522457 Archaea Crenarchaeota Thermoprotei         <NA>          <NA>
#> 3         951 Archaea Crenarchaeota Thermoprotei Sulfolobales Sulfolobaceae
#> 4      244423 Archaea Crenarchaeota        Sd-NA         <NA>          <NA>
#> 5      586076 Archaea Crenarchaeota        Sd-NA         <NA>          <NA>
#> 6      246140 Archaea Crenarchaeota        Sd-NA         <NA>          <NA>
#>        Genus                  Species
#> 1       <NA>                     <NA>
#> 2       <NA>                     <NA>
#> 3 Sulfolobus Sulfolobusacidocaldarius
#> 4       <NA>                     <NA>
#> 5       <NA>                     <NA>
#> 6       <NA>                     <NA>
```

## Preprocessing

The `microbiomedataset` package also includes functions for filtering, subsetting, and merging abundance data.

In the following example, the `global_patterns` data is first transformed to relative abundance, creating the new `global_patterns2` object, which is then filtered such that only OTUs with a mean greater than 10^-5 are kept.


```r
global_patterns2 <-
  global_patterns %>%
  transform2relative_intensity() %>%
  mutate2variable(what = "mean_intensity") %>%
  activate_microbiome_dataset(what = "variable_info") %>%
  filter(mean_intensity > 10 ^ (-5))
```

This results in a highly-subsetted object, `global_patterns2`, containing just 4624 of the original ~19216 OTUs.

Next, only remain the variables that phylum Chlamydiae.


```r
global_patterns_chl <-
  global_patterns %>%
  activate_microbiome_dataset(what = "variable_info") %>%
  dplyr::filter(Phylum == "Chlamydiae")
```

Next, only remain the samples with total intensity > 20.


```r
global_patterns_chl <-
  global_patterns_chl %>%
  mutate2sample(what = "sum_intensity") %>%
  activate_microbiome_dataset(what = "sample_info") %>%
  filter(sum_intensity > 20)
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
