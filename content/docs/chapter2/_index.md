---
date: "2018-09-09T00:00:00Z"
icon: book-reader
icon_pack: fas
linktitle: "Chapter 2-microbiomeDataset class"
summary: "microbiomeDataset package to organize microbiome data."
title: "Chapter 2-microbiomeDataset class"
commentable: true
type: book
weight: 1
---

## About

`microbiomedataset` provide the `microbiome_dataset` class which is specifically developed and designed to organize the rectangular microbiome data sets into a standard structure. `microbiomedataset` package also provide a lot of base processing functions to process and operate the `microbiome_dataset` class. In additional, the microbiome_dataset class can be processed by all the packages from `tidymicrobiome`.

## Install `microbiomedataset`

You can install `microbiomedataset` from GitLab.

```
if(!require(remotes)){
install.packages("remotes")
}
remotes::install_gitlab("tidymicrobiome/microbiomedataset")
```

or GitHub

```
remotes::install_github("tidymicrobiome/microbiomedataset")
```

or tidymicrobiome.org

```
source("https://www.tidymicrobiome.org/tidymicrobiome-packages/install_tidymicrobiome.txt")
install_tidymicrobiome(from = "tidymicrobiome.org", which_package = "microbiomedataset")
```