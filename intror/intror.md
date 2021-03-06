---
layout: post2
permalink: /introduction_to_r_2015/
title: Introduction to R 2015 Student Page
header1: Introduction to R 2015
header2: Workshop pages for students
image: CBW_cancerDNA_icon-16.jpg
---


Laptop Setup Instructions
-------------------------

Instructions for setting up your laptop can be found here: [Laptop Setup Instructions](https://raw.githubusercontent.com/bioinformatics-ca/2015_workshops/master/intror/laptop_instructions.txt)

<hr>

Difference Between R and R Studio

RStudio doesn't know where libraries are installed, when they are not installed through the RStudio package manager. To tell RStudio the location, you can define the path in a startup file. Create a file called `.Renviron` . Inside there:

```
R_LIBS=<R Library Path of other installed packages>
```

That was the problem when students installed things in R Studio at the command line using the **R** command `install.package()`.

... or you could use the package manger to install libraries.

Syntax highlighting  

... of scripts in the **R** editor does not seem to work under Windows. If you want highlighted syntax, use RStudio instead. Or (seriously), get a Mac.

Helpful Materials
-----------------

-   The [Introduction to **R** tutorial](http://steipe.biochemistry.utoronto.ca/abc/index.php/R_tutorial)
-   The [R command cheat sheet](../../resources/R_Short-refcard.pdf)

<hr>
Day 1
-----

<hr>
#### Welcome

<font color="green">*Faculty: Michelle Brazas*</font>

<hr>
#### Module 1: First Steps

<font color="green">*Faculty: Boris Steipe*</font>

**Lecture:**

[2015\_Intro\_to\_R.pdf](‎https://bioinformatics.ca/intror2015module1pdf)  
[2015\_Intro\_to\_R.mp4](‎https://bioinformatics.ca/intror2015module1mp4)  

**Scripts:**

* [**R script template**](https://raw.githubusercontent.com/bioinformatics-ca/bioinformatics-ca.github.io/master/2016_workshops/intror/ScriptTemplate.txt)

-   [**2015\_Intro\_Module\_1\_First\_steps.R**](https://raw.githubusercontent.com/bioinformatics-ca/2015_workshops/master/intror/2015_Intro_Module_1_First_steps.R)

**Data:**

* [**Fig\_3-CharacteristicGenes.txt**](https://raw.githubusercontent.com/bioinformatics-ca/bioinformatics-ca.github.io/master/2016_workshops/intror/Fig_3-CharacteristicGenes.txt)

-   [**Supplementary data: Table\_S3.xls**](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/intror/Table_S3.xls)

**Practical:**

* [**Using Projects with R Studio**](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects)

-   [**Software Carpentry**](http://software-carpentry.org/)
    -   [Best Practices for Scientific Computing, Wilson *et al.*, PLoS Biology, Jan. 2014](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745)
-   [**Version control in R Studio**](https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN)

**Resources:**

* [**Jaitin *et al.* (2014) Single-Cell RNA-seq defines cell types‎**](http://www.ncbi.nlm.nih.gov/pubmed/24531970)

*Help*ful links  

* [The **R** help mailing list](https://stat.ethz.ch/mailman/listinfo/r-help)
* [**Rseek**: the specialized search engine for **R** topics](http://rseek.org/)
* [**R** questions on stackoverflow](http://stackoverflow.com/questions/tagged/r)
* [The Comprehensive **R** Archive Network **CRAN**](http://cran.r-project.org/)
* [The **CRAN** task-view collection](http://cran.r-project.org/web/views/)
* [**Bioconductor** task views](http://www.bioconductor.org/packages/release/BiocViews.html)

<hr>
#### Module 2: Programming Basics

<font color="green">*Faculty: Boris Steipe*</font>

**Practical:**

<hr>
#### Module 3: Using R for Data Analysis

<font color="green">*Faculty: Boris Steipe*</font>

**Scripts:**

* [Scripts](http://bioinformatics-ca.github.io/intror_module2-3_2016/)

**Resources:**

* [Beyond Bar and Line-Graphs](http://www.ncbi.nlm.nih.gov/pubmed/25901488)

**Practical:**

  * [Biological identifier conversion]( http://biodbnet.abcc.ncifcrf.gov/)
  * [Gene co-expression database](http://coxpresdb.jp/) 

<hr>
#### Code Scratchpad

``` r

?read.csv
rawDat <- read.csv("table_S3.csv",
                   header = FALSE,
                   stringsAsFactors = FALSE)
                   
head(rawDat, 10)
rawDat <- rawDat[-(1:6), ]
head(rawDat, 10)   # now note rownames problem
types <- c("genes",
           "B.ctrl",
           "B.LPS",
           "MF.ctrl",
           "MF.LPS",
           "NK.ctrl",
           "NK.LPS",
           "Mo.ctrl",
           "Mo.LPS",
           "pDC.ctrl",
           "pDC.LPS",
           "DC1.ctrl",
           "DC1.LPS",
           "DC2.ctrl",
           "DC2.LPS",
           "cluster")

colnames(rawDat) <- types

# Fix rownames problem
nrow(rawDat)
rownames(rawDat) <- 1:nrow(rawDat)

for (i in 2:ncol(rawDat)) {
  rawDat[,i] <- as.numeric(rawDat[ ,i])
}

typeInfo(rawDat)
```

filtering, preparing data for heatmap  

``` r
sup3 <- read.csv("table_S3.csv",
                  skip = 6,
                  header = FALSE,
                  colClasses = c("character", rep("numeric", 15)),
                  col.names = c("genes",
                                  "B.ctrl", "B.LPS",
                                  "MF.ctrl", "MF.LPS",
                                  "NK.ctrl", "NK.LPS",
                                  "Mo.ctrl", "Mo.LPS",
                                  "pDC.ctrl", "pDC.LPS",
                                  "DC1.ctrl", "DC1.LPS",
                                  "DC2.ctrl", "DC2.LPS",
                                  "cluster"),
                  stringsAsFactors = FALSE)

summary(sup3[ ,2])
head(sup3)
dfMo <- abs(sup3[ ,"Mo.ctrl"] - sup3[ ,"Mo.LPS"]) 
summary(dfMo)

# example of order()
dfMo[1:5]
order(dfMo[1:5])
dfMo[order(dfMo[1:5])]

# for all genes ...
dfMoOrdered <- order(dfMo)
dfGenes <- tail(sup3[dfMoOrdered, 1], 10)

# convert to matrix
sup3M <- as.matrix(sup3[ ,2:15], ncol=14)
heatmap(sup3M)
```
