## Next steps

Data in bioinformatics is often complex. To deal with this, developers
define specialised data containers (termed classes) that match the
properties of the data they need to handle.

This aspect is central to the **Bioconductor**[1] project which uses the
same **core data infrastructure** across packages. This certainly
contributed to Bioconductor’s success. Bioconductor package developers
are advised to make use of existing infrastructure to provide coherence,
interoperability, and stability to the project as a whole.

To illustrate such an omics data container, we’ll present the
`SummarizedExperiment` class.

## SummarizedExperiment

The figure below represents the anatomy of the SummarizedExperiment
class.

<img src="https://uclouvain-cbio.github.io/WSBIM1322/figs/SE.svg" width="80%" style="display: block; margin: auto;" />

Objects of the class SummarizedExperiment contain :

-   **One (or more) assay(s)** containing the quantitative omics data
    (expression data), stored as a matrix-like object. Features (genes,
    transcripts, proteins, …) are defined along the rows, and samples
    along the columns.

-   A **sample metadata** slot containing sample co-variates, stored as
    a data frame. Rows from this table represent samples (rows match
    exactly the columns of the expression data).

-   A **feature metadata** slot containing feature co-variates, stored
    as a data frame. The rows of this data frame match exactly the rows
    of the expression data.

The coordinated nature of the SummarizedExperiment guarantees that
during data manipulation, the dimensions of the different slots will
always match (i.e the columns in the expression data and then rows in
the sample metadata, as well as the rows in the expression data and
feature metadata) during data manipulation. For example, if we had to
exclude one sample from the assay, it would be automatically removed
from the sample metadata in the same operation.

The metadata slots can grow additional co-variates (columns) without
affecting the other structures.

### Creating a SummarizedExperiment

Remember the `rna` dataset that we have used previously.

From this table, we have already created 3 different tables.

-   **An expression matrix**

<!-- -->

    count_matrix[1:5, ]

{: .language-r}

            GSM2545336 GSM2545337 GSM2545338 GSM2545339 GSM2545340 GSM2545341 GSM2545342 GSM2545343 GSM2545344 GSM2545345
    Asl           1170        361        400        586        626        988        836        535        586        597
    Apod         36194      10347       9173      10620      13021      29594      24959      13668      13230      15868
    Cyp2d22       4060       1616       1603       1901       2171       3349       3122       2008       2254       2277
    Klk6           287        629        641        578        448        195        186       1101        537        567
    Fcrls           85        233        244        237        180         38         68        375        199        177
            GSM2545346 GSM2545347 GSM2545348 GSM2545349 GSM2545350 GSM2545351 GSM2545352 GSM2545353 GSM2545354 GSM2545362
    Asl            938       1035        494        481        666        937        803        541        473        748
    Apod         27769      34301      11258      11812      15816      29242      20415      13682      11088      15916
    Cyp2d22       2985       3452       1883       2014       2417       3678       2920       2216       1821       2842
    Klk6           327        233        742        881        828        250        798        710        894        501
    Fcrls           89         67        300        233        231         81        303        285        248        179
            GSM2545363 GSM2545380
    Asl            576       1192
    Apod         11166      38148
    Cyp2d22       2011       4019
    Klk6           598        259
    Fcrls          184         68

{: .output}

    dim(count_matrix)

{: .language-r}

    [1] 1474   22

{: .output}

-   **A table describing the samples**

<!-- -->

    sample_metadata

{: .language-r}

    # A tibble: 22 × 9
       sample     organism       age sex    infection   strain   time tissue     mouse
       <chr>      <chr>        <dbl> <chr>  <chr>       <chr>   <dbl> <chr>      <dbl>
     1 GSM2545336 Mus musculus     8 Female InfluenzaA  C57BL/6     8 Cerebellum    14
     2 GSM2545337 Mus musculus     8 Female NonInfected C57BL/6     0 Cerebellum     9
     3 GSM2545338 Mus musculus     8 Female NonInfected C57BL/6     0 Cerebellum    10
     4 GSM2545339 Mus musculus     8 Female InfluenzaA  C57BL/6     4 Cerebellum    15
     5 GSM2545340 Mus musculus     8 Male   InfluenzaA  C57BL/6     4 Cerebellum    18
     6 GSM2545341 Mus musculus     8 Male   InfluenzaA  C57BL/6     8 Cerebellum     6
     7 GSM2545342 Mus musculus     8 Female InfluenzaA  C57BL/6     8 Cerebellum     5
     8 GSM2545343 Mus musculus     8 Male   NonInfected C57BL/6     0 Cerebellum    11
     9 GSM2545344 Mus musculus     8 Female InfluenzaA  C57BL/6     4 Cerebellum    22
    10 GSM2545345 Mus musculus     8 Male   InfluenzaA  C57BL/6     4 Cerebellum    13
    # … with 12 more rows

{: .output}

-   **A table describing the genes**

<!-- -->

    gene_metadata

{: .language-r}

    # A tibble: 1,474 × 9
       gene    ENTREZID product                                                 ensem…¹ exter…² chrom…³ gene_…⁴ pheno…⁵ hsapi…⁶
       <chr>      <dbl> <chr>                                                   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
     1 Asl       109900 argininosuccinate lyase, transcript variant X1          ENSMUS… 251000… 5       protei… abnorm… ASL    
     2 Apod       11815 apolipoprotein D, transcript variant 3                  ENSMUS… <NA>    16      protei… abnorm… APOD   
     3 Cyp2d22    56448 cytochrome P450, family 2, subfamily d, polypeptide 22… ENSMUS… 2D22    15      protei… abnorm… CYP2D6 
     4 Klk6       19144 kallikrein related-peptidase 6, transcript variant 2    ENSMUS… Bssp    7       protei… abnorm… KLK6   
     5 Fcrls      80891 Fc receptor-like S, scavenger receptor, transcript var… ENSMUS… 281043… 3       protei… decrea… FCRL2  
     6 Slc2a4     20528 solute carrier family 2 (facilitated glucose transport… ENSMUS… Glut-4  11      protei… abnorm… SLC2A4 
     7 Exd2       97827 exonuclease 3'-5' domain containing 2                   ENSMUS… 493053… 12      protei… <NA>    EXD2   
     8 Gjc2      118454 gap junction protein, gamma 2, transcript variant 1     ENSMUS… B23038… 11      protei… Purkin… GJC2   
     9 Plp1       18823 proteolipid protein (myelin) 1, transcript variant 1    ENSMUS… DM20    X       protei… abnorm… PLP1   
    10 Gnb4       14696 guanine nucleotide binding protein (G protein), beta 4… ENSMUS… 672045… 3       protei… decrea… GNB4   
    # … with 1,464 more rows, and abbreviated variable names ¹​ensembl_gene_id, ²​external_synonym, ³​chromosome_name,
    #   ⁴​gene_biotype, ⁵​phenotype_description, ⁶​hsapiens_homolog_associated_gene_name

{: .output}

We will create a `SummarizedExperiment` from these tables:

-   The count matrix that will be used as the **`assay`**

-   The table describing the samples will be used as the **sample
    metadata** slot

-   The table describing the genes will be used as the **features
    metadata** slot

To do this we can put the different parts together using the
`SummarizedExperiment` constructor:

    #BiocManager::install("SummarizedExperiment")
    library("SummarizedExperiment")

{: .language-r}

    se <- SummarizedExperiment(assays = count_matrix,
                               colData = sample_metadata,
                               rowData = gene_metadata)
    se

{: .language-r}

    class: SummarizedExperiment 
    dim: 1474 22 
    metadata(0):
    assays(1): ''
    rownames(1474): Asl Apod ... Lmx1a Pbx1
    rowData names(9): gene ENTREZID ... phenotype_description hsapiens_homolog_associated_gene_name
    colnames(22): GSM2545336 GSM2545337 ... GSM2545363 GSM2545380
    colData names(9): sample organism ... tissue mouse

{: .output}

Using this data structure, we can access the expression matrix with the
`assay` function:

    head(assay(se))

{: .language-r}

            GSM2545336 GSM2545337 GSM2545338 GSM2545339 GSM2545340 GSM2545341 GSM2545342 GSM2545343 GSM2545344 GSM2545345
    Asl           1170        361        400        586        626        988        836        535        586        597
    Apod         36194      10347       9173      10620      13021      29594      24959      13668      13230      15868
    Cyp2d22       4060       1616       1603       1901       2171       3349       3122       2008       2254       2277
    Klk6           287        629        641        578        448        195        186       1101        537        567
    Fcrls           85        233        244        237        180         38         68        375        199        177
    Slc2a4         782        231        248        265        313        786        528        249        266        357
            GSM2545346 GSM2545347 GSM2545348 GSM2545349 GSM2545350 GSM2545351 GSM2545352 GSM2545353 GSM2545354 GSM2545362
    Asl            938       1035        494        481        666        937        803        541        473        748
    Apod         27769      34301      11258      11812      15816      29242      20415      13682      11088      15916
    Cyp2d22       2985       3452       1883       2014       2417       3678       2920       2216       1821       2842
    Klk6           327        233        742        881        828        250        798        710        894        501
    Fcrls           89         67        300        233        231         81        303        285        248        179
    Slc2a4         654        693        271        304        349        715        513        320        248        350
            GSM2545363 GSM2545380
    Asl            576       1192
    Apod         11166      38148
    Cyp2d22       2011       4019
    Klk6           598        259
    Fcrls          184         68
    Slc2a4         317        796

{: .output}

    dim(assay(se))

{: .language-r}

    [1] 1474   22

{: .output}

We can access the sample metadata using the `colData` function:

    colData(se)

{: .language-r}

    DataFrame with 22 rows and 9 columns
                    sample     organism       age         sex   infection      strain      time      tissue     mouse
               <character>  <character> <numeric> <character> <character> <character> <numeric> <character> <numeric>
    GSM2545336  GSM2545336 Mus musculus         8      Female  InfluenzaA     C57BL/6         8  Cerebellum        14
    GSM2545337  GSM2545337 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         9
    GSM2545338  GSM2545338 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum        10
    GSM2545339  GSM2545339 Mus musculus         8      Female  InfluenzaA     C57BL/6         4  Cerebellum        15
    GSM2545340  GSM2545340 Mus musculus         8        Male  InfluenzaA     C57BL/6         4  Cerebellum        18
    ...                ...          ...       ...         ...         ...         ...       ...         ...       ...
    GSM2545353  GSM2545353 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         4
    GSM2545354  GSM2545354 Mus musculus         8        Male NonInfected     C57BL/6         0  Cerebellum         2
    GSM2545362  GSM2545362 Mus musculus         8      Female  InfluenzaA     C57BL/6         4  Cerebellum        20
    GSM2545363  GSM2545363 Mus musculus         8        Male  InfluenzaA     C57BL/6         4  Cerebellum        12
    GSM2545380  GSM2545380 Mus musculus         8      Female  InfluenzaA     C57BL/6         8  Cerebellum        19

{: .output}

    dim(colData(se))

{: .language-r}

    [1] 22  9

{: .output}

We can also access the feature metadata using the `rowData` function:

    head(rowData(se))

{: .language-r}

    DataFrame with 6 rows and 9 columns
                   gene  ENTREZID                product    ensembl_gene_id external_synonym chromosome_name   gene_biotype
            <character> <numeric>            <character>        <character>      <character>     <character>    <character>
    Asl             Asl    109900 argininosuccinate ly.. ENSMUSG00000025533    2510006M18Rik               5 protein_coding
    Apod           Apod     11815 apolipoprotein D, tr.. ENSMUSG00000022548               NA              16 protein_coding
    Cyp2d22     Cyp2d22     56448 cytochrome P450, fam.. ENSMUSG00000061740             2D22              15 protein_coding
    Klk6           Klk6     19144 kallikrein related-p.. ENSMUSG00000050063             Bssp               7 protein_coding
    Fcrls         Fcrls     80891 Fc receptor-like S, .. ENSMUSG00000015852    2810439C17Rik               3 protein_coding
    Slc2a4       Slc2a4     20528 solute carrier famil.. ENSMUSG00000018566           Glut-4              11 protein_coding
             phenotype_description hsapiens_homolog_associated_gene_name
                       <character>                           <character>
    Asl     abnormal circulating..                                   ASL
    Apod    abnormal lipid homeo..                                  APOD
    Cyp2d22 abnormal skin morpho..                                CYP2D6
    Klk6    abnormal cytokine le..                                  KLK6
    Fcrls   decreased CD8-positi..                                 FCRL2
    Slc2a4  abnormal circulating..                                SLC2A4

{: .output}

    dim(rowData(se))

{: .language-r}

    [1] 1474    9

{: .output}

### Subsetting a SummarizedExperiment

SummarizedExperiment can be subset just like with data frames, with
numerics or with characters of logicals.

Below, we create a new instance of class SummarizedExperiment that
contains only the 5 first features for the 3 first samples.

    se1 <- se[1:5, 1:3]
    se1

{: .language-r}

    class: SummarizedExperiment 
    dim: 5 3 
    metadata(0):
    assays(1): ''
    rownames(5): Asl Apod Cyp2d22 Klk6 Fcrls
    rowData names(9): gene ENTREZID ... phenotype_description hsapiens_homolog_associated_gene_name
    colnames(3): GSM2545336 GSM2545337 GSM2545338
    colData names(9): sample organism ... tissue mouse

{: .output}

    colData(se1)

{: .language-r}

    DataFrame with 3 rows and 9 columns
                    sample     organism       age         sex   infection      strain      time      tissue     mouse
               <character>  <character> <numeric> <character> <character> <character> <numeric> <character> <numeric>
    GSM2545336  GSM2545336 Mus musculus         8      Female  InfluenzaA     C57BL/6         8  Cerebellum        14
    GSM2545337  GSM2545337 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         9
    GSM2545338  GSM2545338 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum        10

{: .output}

    rowData(se1)

{: .language-r}

    DataFrame with 5 rows and 9 columns
                   gene  ENTREZID                product    ensembl_gene_id external_synonym chromosome_name   gene_biotype
            <character> <numeric>            <character>        <character>      <character>     <character>    <character>
    Asl             Asl    109900 argininosuccinate ly.. ENSMUSG00000025533    2510006M18Rik               5 protein_coding
    Apod           Apod     11815 apolipoprotein D, tr.. ENSMUSG00000022548               NA              16 protein_coding
    Cyp2d22     Cyp2d22     56448 cytochrome P450, fam.. ENSMUSG00000061740             2D22              15 protein_coding
    Klk6           Klk6     19144 kallikrein related-p.. ENSMUSG00000050063             Bssp               7 protein_coding
    Fcrls         Fcrls     80891 Fc receptor-like S, .. ENSMUSG00000015852    2810439C17Rik               3 protein_coding
             phenotype_description hsapiens_homolog_associated_gene_name
                       <character>                           <character>
    Asl     abnormal circulating..                                   ASL
    Apod    abnormal lipid homeo..                                  APOD
    Cyp2d22 abnormal skin morpho..                                CYP2D6
    Klk6    abnormal cytokine le..                                  KLK6
    Fcrls   decreased CD8-positi..                                 FCRL2

{: .output}

We can also use the `colData()` function to subset on something from the
sample metadata or the `rowData()` to subset on something from the
feature metadata. For example, here we keep only miRNAs and the non
infected samples:

    se1 <- se[rowData(se)$gene_biotype == "miRNA",
              colData(se)$infection == "NonInfected"]
    se1

{: .language-r}

    class: SummarizedExperiment 
    dim: 7 7 
    metadata(0):
    assays(1): ''
    rownames(7): Mir1901 Mir378a ... Mir128-1 Mir7682
    rowData names(9): gene ENTREZID ... phenotype_description hsapiens_homolog_associated_gene_name
    colnames(7): GSM2545337 GSM2545338 ... GSM2545353 GSM2545354
    colData names(9): sample organism ... tissue mouse

{: .output}

    assay(se1)

{: .language-r}

             GSM2545337 GSM2545338 GSM2545343 GSM2545348 GSM2545349 GSM2545353 GSM2545354
    Mir1901          45         44         74         55         68         33         60
    Mir378a          11          7          9          4         12          4          8
    Mir133b           4          6          5          4          6          7          3
    Mir30c-2         10          6         16         12          8         17         15
    Mir149            1          2          0          0          0          0          2
    Mir128-1          4          1          2          2          1          2          1
    Mir7682           2          0          4          1          3          5          5

{: .output}

    colData(se1)

{: .language-r}

    DataFrame with 7 rows and 9 columns
                    sample     organism       age         sex   infection      strain      time      tissue     mouse
               <character>  <character> <numeric> <character> <character> <character> <numeric> <character> <numeric>
    GSM2545337  GSM2545337 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         9
    GSM2545338  GSM2545338 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum        10
    GSM2545343  GSM2545343 Mus musculus         8        Male NonInfected     C57BL/6         0  Cerebellum        11
    GSM2545348  GSM2545348 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         8
    GSM2545349  GSM2545349 Mus musculus         8        Male NonInfected     C57BL/6         0  Cerebellum         7
    GSM2545353  GSM2545353 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         4
    GSM2545354  GSM2545354 Mus musculus         8        Male NonInfected     C57BL/6         0  Cerebellum         2

{: .output}

    rowData(se1)

{: .language-r}

    DataFrame with 7 rows and 9 columns
                    gene  ENTREZID        product    ensembl_gene_id external_synonym chromosome_name gene_biotype
             <character> <numeric>    <character>        <character>      <character>     <character>  <character>
    Mir1901      Mir1901 100316686  microRNA 1901 ENSMUSG00000084565         Mirn1901              18        miRNA
    Mir378a      Mir378a    723889  microRNA 378a ENSMUSG00000105200          Mirn378              18        miRNA
    Mir133b      Mir133b    723817  microRNA 133b ENSMUSG00000065480         mir 133b               1        miRNA
    Mir30c-2    Mir30c-2    723964 microRNA 30c-2 ENSMUSG00000065567        mir 30c-2               1        miRNA
    Mir149        Mir149    387167   microRNA 149 ENSMUSG00000065470          Mirn149               1        miRNA
    Mir128-1    Mir128-1    387147 microRNA 128-1 ENSMUSG00000065520          Mirn128               1        miRNA
    Mir7682      Mir7682 102466847  microRNA 7682 ENSMUSG00000106406     mmu-mir-7682               1        miRNA
              phenotype_description hsapiens_homolog_associated_gene_name
                        <character>                           <character>
    Mir1901                      NA                                    NA
    Mir378a  abnormal mitochondri..                               MIR378A
    Mir133b  no abnormal phenotyp..                               MIR133B
    Mir30c-2                     NA                               MIR30C2
    Mir149   increased circulatin..                                    NA
    Mir128-1 no abnormal phenotyp..                              MIR128-1
    Mir7682                      NA                                    NA

{: .output}

For the following exercise, you should download the SE.rda object (that
contains the `se` object), and open the file using the ‘load()’
function.

    download.file(url = "https://raw.githubusercontent.com/UCLouvain-CBIO/bioinfo-training-01-intro-r/master/data/SE.rda",
                  destfile = "data/SE.rda")

    load(file = "data/SE.rda")

{: .language-r}

> ## Challenge
>
> Extract the gene expression levels of the 3 first genes in samples at
> time 0 and at time 8.
>
> > ## Solution
> >
> >     assay(se)[1:3, colData(se)$time != 4]
> >
> > {: .language-r}
> >
> >             GSM2545336 GSM2545337 GSM2545338 GSM2545341 GSM2545342 GSM2545343 GSM2545346 GSM2545347 GSM2545348 GSM2545349
> >     Asl           1170        361        400        988        836        535        938       1035        494        481
> >     Apod         36194      10347       9173      29594      24959      13668      27769      34301      11258      11812
> >     Cyp2d22       4060       1616       1603       3349       3122       2008       2985       3452       1883       2014
> >             GSM2545351 GSM2545353 GSM2545354 GSM2545380
> >     Asl            937        541        473       1192
> >     Apod         29242      13682      11088      38148
> >     Cyp2d22       3678       2216       1821       4019
> >
> > {: .output}
> >
> >     # Equivalent to
> >     assay(se)[1:3, colData(se)$time == 0 | colData(se)$time == 8]
> >
> > {: .language-r}
> >
> >             GSM2545336 GSM2545337 GSM2545338 GSM2545341 GSM2545342 GSM2545343 GSM2545346 GSM2545347 GSM2545348 GSM2545349
> >     Asl           1170        361        400        988        836        535        938       1035        494        481
> >     Apod         36194      10347       9173      29594      24959      13668      27769      34301      11258      11812
> >     Cyp2d22       4060       1616       1603       3349       3122       2008       2985       3452       1883       2014
> >             GSM2545351 GSM2545353 GSM2545354 GSM2545380
> >     Asl            937        541        473       1192
> >     Apod         29242      13682      11088      38148
> >     Cyp2d22       3678       2216       1821       4019
> >
> > {: .output} {: .solution} {: .challenge}

#### Adding variables to metadata

We can also add information to the metadata. Suppose that you want to
add the center where the samples were collected…

    colData(se)$center <- rep("University of Illinois", nrow(colData(se)))
    colData(se)

{: .language-r}

    DataFrame with 22 rows and 10 columns
                    sample     organism       age         sex   infection      strain      time      tissue     mouse
               <character>  <character> <numeric> <character> <character> <character> <numeric> <character> <numeric>
    GSM2545336  GSM2545336 Mus musculus         8      Female  InfluenzaA     C57BL/6         8  Cerebellum        14
    GSM2545337  GSM2545337 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         9
    GSM2545338  GSM2545338 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum        10
    GSM2545339  GSM2545339 Mus musculus         8      Female  InfluenzaA     C57BL/6         4  Cerebellum        15
    GSM2545340  GSM2545340 Mus musculus         8        Male  InfluenzaA     C57BL/6         4  Cerebellum        18
    ...                ...          ...       ...         ...         ...         ...       ...         ...       ...
    GSM2545353  GSM2545353 Mus musculus         8      Female NonInfected     C57BL/6         0  Cerebellum         4
    GSM2545354  GSM2545354 Mus musculus         8        Male NonInfected     C57BL/6         0  Cerebellum         2
    GSM2545362  GSM2545362 Mus musculus         8      Female  InfluenzaA     C57BL/6         4  Cerebellum        20
    GSM2545363  GSM2545363 Mus musculus         8        Male  InfluenzaA     C57BL/6         4  Cerebellum        12
    GSM2545380  GSM2545380 Mus musculus         8      Female  InfluenzaA     C57BL/6         8  Cerebellum        19
                               center
                          <character>
    GSM2545336 University of Illinois
    GSM2545337 University of Illinois
    GSM2545338 University of Illinois
    GSM2545339 University of Illinois
    GSM2545340 University of Illinois
    ...                           ...
    GSM2545353 University of Illinois
    GSM2545354 University of Illinois
    GSM2545362 University of Illinois
    GSM2545363 University of Illinois
    GSM2545380 University of Illinois

{: .output}

This illustrates that the metadata slots can grow indefinitely without
affecting the other structures!

**Take-home message**

-   `SummarizedExperiment` represents an efficient way to store and
    handle omics data.

-   They are used in many Bioconductor packages.

If you follow the next training focused on RNA sequencing analysis, you
will learn to use the Bioconductor `DESeq2` package to do some
differential expression analyses. The whole analysis of the `DESeq2`
package is handled in a `SummarizedExperiment`.

[1] The [Bioconductor](http://www.bioconductor.org) was initiated by
Robert Gentleman, one of the two creators of the R language.
Bioconductor provides tools dedicated to omics data analysis.
Bioconductor uses the R statistical programming language and is open
source and open development.
