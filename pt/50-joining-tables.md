## Joining tables

In many real life situations, data are spread across multiple tables. Usually this occurs because different types of information about a subject, e.g. a patient, are collected from different sources.

It may be desirable for some analyses to combine data from two or more tables into a single data frame based on a column that would be common to all the tables, for example, an attribute that uniquely identifies the subjects.

The `dplyr` package provides a set of join functions for combining two data frames based on matches within specified columns.

For further reading, please refer to the chapter about [table joins](https://r4ds.had.co.nz/relational-data.html#understanding-joins) in [R for Data Science](https://r4ds.had.co.nz/).

The [Data Transformation Cheat Sheet](https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf) also provides a short overview on table joins.

## Combining tables

We are going to illustrate join using a common example from the bioinformatics world, where annotations about genes are scattered in different tables that have one or more shared columns.

The data we are going to use are available in the following package.

    if (!require("rWSBIM1207"))
        BiocManager::install("UCLouvain-CBIO/rWSBIM1207")

{: .language-r}

    library("rWSBIM1207")
    data(jdf)

{: .language-r}

The data is composed of several tables.

The first table, `jdf1`, contains protein [UniProt](https://www.uniprot.org/)[1] unique accession number (`uniprot` variable), the most likely sub-cellular localisation of these respective proteins (`organelle` variable) as well as the proteins identifier (`entry`).

    jdf1

{: .language-r}

    # A tibble: 25 × 3
       uniprot  organelle                             entry      
       <chr>    <chr>                                 <chr>      
     1 P26039   Actin cytoskeleton                    TLN1_MOUSE 
     2 Q99PL5   Endoplasmic reticulum/Golgi apparatus RRBP1_MOUSE
     3 Q6PB66   Mitochondrion                         LPPRC_MOUSE
     4 P11276   Extracellular matrix                  FINC_MOUSE 
     5 Q6PR54   Nucleus - Chromatin                   RIF1_MOUSE 
     6 Q05793   Extracellular matrix                  PGBM_MOUSE 
     7 P19096   Cytosol                               FAS_MOUSE  
     8 Q9JKF1   Plasma membrane                       IQGA1_MOUSE
     9 Q9QZQ1-2 Plasma membrane                       AFAD_MOUSE 
    10 Q6NS46   Nucleus - Non-chromatin               RRP5_MOUSE 
    # … with 15 more rows

{: .output}

The second table, `jdf2`, contains the name of the gene that codes for the protein (`gene_name` variable), a description of the gene (`description` variable), the uniprot accession number (this is the common variable that can be used to join tables) and the species the protein information comes from (`organism` variable).

    jdf2

{: .language-r}

    # A tibble: 25 × 4
       gene_name description                                      uniprot organism
       <chr>     <chr>                                            <chr>   <chr>   
     1 Iqgap1    Ras GTPase-activating-like protein IQGAP1        Q9JKF1  Mmus    
     2 Hspa5     78 kDa glucose-regulated protein                 P20029  Mmus    
     3 Pdcd11    Protein RRP5 homolog                             Q6NS46  Mmus    
     4 Tfrc      Transferrin receptor protein 1                   Q62351  Mmus    
     5 Hspd1     60 kDa heat shock protein, mitochondrial         P63038  Mmus    
     6 Tln1      Talin-1                                          P26039  Mmus    
     7 Smc1a     Structural maintenance of chromosomes protein 1A Q9CU62  Mmus    
     8 Lamc1     Laminin subunit gamma-1                          P02468  Mmus    
     9 Hsp90b1   Endoplasmin                                      P08113  Mmus    
    10 Mia3      Melanoma inhibitory activity protein 3           Q8BI84  Mmus    
    # … with 15 more rows

{: .output}

We now want to join these two tables into a single one containing all variables.

We are going to use the `full_join` function of `dplyr` to do so,

Th function will automatically find the common variable (in this case `uniprot`) to match observations from the first and second table.

    library("dplyr")

{: .language-r}

     次のパッケージを付け加えます: 'dplyr'

{: .output}

     以下のオブジェクトは 'package:stats' からマスクされています:
    
        filter, lag

{: .output}

     以下のオブジェクトは 'package:base' からマスクされています:
    
        intersect, setdiff, setequal, union

{: .output}

    full_join(jdf1, jdf2)

{: .language-r}

    Joining, by = "uniprot"

{: .output}

    # A tibble: 25 × 6
       uniprot  organelle                             entry       gene_name descr…¹ organ…²
       <chr>    <chr>                                 <chr>       <chr>     <chr>   <chr>  
     1 P26039   Actin cytoskeleton                    TLN1_MOUSE  Tln1      Talin-1 Mmus   
     2 Q99PL5   Endoplasmic reticulum/Golgi apparatus RRBP1_MOUSE Rrbp1     Riboso… Mmus   
     3 Q6PB66   Mitochondrion                         LPPRC_MOUSE Lrpprc    Leucin… Mmus   
     4 P11276   Extracellular matrix                  FINC_MOUSE  Fn1       Fibron… Mmus   
     5 Q6PR54   Nucleus - Chromatin                   RIF1_MOUSE  Rif1      Telome… Mmus   
     6 Q05793   Extracellular matrix                  PGBM_MOUSE  Hspg2     Baseme… Mmus   
     7 P19096   Cytosol                               FAS_MOUSE   Fasn      Fatty … Mmus   
     8 Q9JKF1   Plasma membrane                       IQGA1_MOUSE Iqgap1    Ras GT… Mmus   
     9 Q9QZQ1-2 Plasma membrane                       AFAD_MOUSE  Mllt4     Isofor… Mmus   
    10 Q6NS46   Nucleus - Non-chromatin               RRP5_MOUSE  Pdcd11    Protei… Mmus   
    # … with 15 more rows, and abbreviated variable names ¹​description, ²​organism

{: .output}

In these examples, each observation of the `jdf1` and `jdf2` tables are uniquely identified by their UniProt accession number. Such variables are called **keys**. Keys are used to match observations across different tables.

Now let’s look at a third table, `jdf3`. It also contains the column uniProt, but it is written differently!

    jdf3

{: .language-r}

    # A tibble: 25 × 4
       gene_name description                                      UniProt organism
       <chr>     <chr>                                            <chr>   <chr>   
     1 Iqgap1    Ras GTPase-activating-like protein IQGAP1        Q9JKF1  Mmus    
     2 Hspa5     78 kDa glucose-regulated protein                 P20029  Mmus    
     3 Pdcd11    Protein RRP5 homolog                             Q6NS46  Mmus    
     4 Tfrc      Transferrin receptor protein 1                   Q62351  Mmus    
     5 Hspd1     60 kDa heat shock protein, mitochondrial         P63038  Mmus    
     6 Tln1      Talin-1                                          P26039  Mmus    
     7 Smc1a     Structural maintenance of chromosomes protein 1A Q9CU62  Mmus    
     8 Lamc1     Laminin subunit gamma-1                          P02468  Mmus    
     9 Hsp90b1   Endoplasmin                                      P08113  Mmus    
    10 Mia3      Melanoma inhibitory activity protein 3           Q8BI84  Mmus    
    # … with 15 more rows

{: .output}

In case none of the variable names match, we can set manually the variables to use for the matching. These variables can be set using the `by` argument, as shown below with the `jdf1` (as above) and `jdf3` tables, where the UniProt accession number is encoded using a different capitalisation.

    names(jdf3)

{: .language-r}

    [1] "gene_name"   "description" "UniProt"     "organism"

{: .output}

    full_join(jdf1, jdf3, by = c("uniprot" = "UniProt"))

{: .language-r}

    # A tibble: 25 × 6
       uniprot  organelle                             entry       gene_name descr…¹ organ…²
       <chr>    <chr>                                 <chr>       <chr>     <chr>   <chr>  
     1 P26039   Actin cytoskeleton                    TLN1_MOUSE  Tln1      Talin-1 Mmus   
     2 Q99PL5   Endoplasmic reticulum/Golgi apparatus RRBP1_MOUSE Rrbp1     Riboso… Mmus   
     3 Q6PB66   Mitochondrion                         LPPRC_MOUSE Lrpprc    Leucin… Mmus   
     4 P11276   Extracellular matrix                  FINC_MOUSE  Fn1       Fibron… Mmus   
     5 Q6PR54   Nucleus - Chromatin                   RIF1_MOUSE  Rif1      Telome… Mmus   
     6 Q05793   Extracellular matrix                  PGBM_MOUSE  Hspg2     Baseme… Mmus   
     7 P19096   Cytosol                               FAS_MOUSE   Fasn      Fatty … Mmus   
     8 Q9JKF1   Plasma membrane                       IQGA1_MOUSE Iqgap1    Ras GT… Mmus   
     9 Q9QZQ1-2 Plasma membrane                       AFAD_MOUSE  Mllt4     Isofor… Mmus   
    10 Q6NS46   Nucleus - Non-chromatin               RRP5_MOUSE  Pdcd11    Protei… Mmus   
    # … with 15 more rows, and abbreviated variable names ¹​description, ²​organism

{: .output}

As can be seen above, the variable name of the first table is retained in the joined one.

> ## Challenge
> 
> Using the `full_join` function, join tables `jdf4` and `jdf5`. What has happened for observations `P26039` and `P02468`?
> 
> > ## Solution
> > 
> > full_join(jdf4, jdf5) 
    {: .language-r}
 Joining, by = "uniprot" 
    {: .output}
 # A tibble: 14 × 6 uniprot  organelle                             entry       gene_name descr…¹ organ…² <chr>    <chr>                                 <chr>       <chr>     <chr>   <chr>  
> > 1 P26039   Actin cytoskeleton                    TLN1_MOUSE  <NA>      <NA>    <NA>   
> > 2 Q99PL5   Endoplasmic reticulum/Golgi apparatus RRBP1_MOUSE <NA>      <NA>    <NA>   
> > 3 Q6PB66   Mitochondrion                         LPPRC_MOUSE <NA>      <NA>    <NA>   
> > 4 P11276   Extracellular matrix                  FINC_MOUSE  <NA>      <NA>    <NA>   
> > 5 Q6PR54   Nucleus - Chromatin                   RIF1_MOUSE  <NA>      <NA>    <NA>   
> > 6 Q05793   Extracellular matrix                  PGBM_MOUSE  <NA>      <NA>    <NA>   
> > 7 P19096   Cytosol                               FAS_MOUSE   Fasn      Fatty … Mmus   
> > 8 Q9JKF1   Plasma membrane                       IQGA1_MOUSE <NA>      <NA>    <NA>   
> > 9 Q9QZQ1-2 Plasma membrane                       AFAD_MOUSE  <NA>      <NA>    <NA>   
> > 10 Q6NS46   Nucleus - Non-chromatin               RRP5_MOUSE  <NA>      <NA>    <NA>   
> > 11 P02468   <NA>                                  <NA>        Lamc1     Lamini… Mmus   
> > 12 P08113   <NA>                                  <NA>        Hsp90b1   Endopl… Mmus   
> > 13 Q8BI84   <NA>                                  <NA>        Mia3      Melano… Mmus   
> > 14 Q6P5D8   <NA>                                  <NA>        Smchd1    Struct… Mmus   
> > # … with abbreviated variable names ¹​description, ²​organism 
    {: .output}
 `P26039` and `P02468` are only present in `jdf4` and `jdf5` respectively, and their respective values for the variables of the table have been encoded as missing.
    
    {: .solution} {: .challenge}

## Different types of joins

Above, we have used the `full_join` function, that fully joins two tables and keeps all observations, adding missing values if necessary. Sometimes, we want to be selective, and keep observations that are present in only one or both tables.

-   An **inner join** keeps observations that are present in both tables.

<img src="../fig/join-inner.png" alt="An inner join matches pairs of observation matching in both tables, this dropping those that are unique to one table. Figure taken from *R for Data Science*." width="70%" />
<p class="caption">
An inner join matches pairs of observation matching in both tables, this
dropping those that are unique to one table. Figure taken from *R for
Data Science*.
</p>

-   A **left join** keeps observations that are present in the left (first) table, dropping those that are only present in the other.
-   A **right join** keeps observations that are present in the right (second) table, dropping those that are only present in the other.
-   A **full join** keeps all observations.

<img src="../fig/join-outer.png" alt="Outer joins match observations that appear in at least on table, filling up missing values with `NA` values. Figure taken from *R for Data Science*." width="70%" />
<p class="caption">
Outer joins match observations that appear in at least on table, filling
up missing values with `NA` values. Figure taken from *R for Data
Science*.
</p>

> ## Challenge
> 
> Join tables `jdf4` and `jdf5`, keeping only observations in `jdf4`.
> 
> > ## Solution
> > 
> > left_join(jdf4, jdf5) 
    {: .language-r}
 Joining, by = "uniprot" 
    {: .output}
 # A tibble: 10 × 6 uniprot  organelle                             entry       gene_name descr…¹ organ…² <chr>    <chr>                                 <chr>       <chr>     <chr>   <chr>  
> > 1 P26039   Actin cytoskeleton                    TLN1_MOUSE  <NA>      <NA>    <NA>   
> > 2 Q99PL5   Endoplasmic reticulum/Golgi apparatus RRBP1_MOUSE <NA>      <NA>    <NA>   
> > 3 Q6PB66   Mitochondrion                         LPPRC_MOUSE <NA>      <NA>    <NA>   
> > 4 P11276   Extracellular matrix                  FINC_MOUSE  <NA>      <NA>    <NA>   
> > 5 Q6PR54   Nucleus - Chromatin                   RIF1_MOUSE  <NA>      <NA>    <NA>   
> > 6 Q05793   Extracellular matrix                  PGBM_MOUSE  <NA>      <NA>    <NA>   
> > 7 P19096   Cytosol                               FAS_MOUSE   Fasn      Fatty … Mmus   
> > 8 Q9JKF1   Plasma membrane                       IQGA1_MOUSE <NA>      <NA>    <NA>   
> > 9 Q9QZQ1-2 Plasma membrane                       AFAD_MOUSE  <NA>      <NA>    <NA>   
> > 10 Q6NS46   Nucleus - Non-chromatin               RRP5_MOUSE  <NA>      <NA>    <NA>   
> > # … with abbreviated variable names ¹​description, ²​organism
    
    {: .output} {: .solution} {: .challenge}

> ## Challenge
> 
> Join tables `jdf4` and `jdf5`, keeping only observations in `jdf5`.
> 
> > ## Solution
> > 
> > right_join(jdf4, jdf5) 
    {: .language-r}
 Joining, by = "uniprot" 
    {: .output}
 # A tibble: 5 × 6 uniprot organelle entry     gene_name description                             organ…¹ <chr>   <chr>     <chr>     <chr>     <chr>                                   <chr>  
> > 1 P19096  Cytosol   FAS_MOUSE Fasn      Fatty acid synthase                     Mmus   
> > 2 P02468  <NA>      <NA>      Lamc1     Laminin subunit gamma-1                 Mmus   
> > 3 P08113  <NA>      <NA>      Hsp90b1   Endoplasmin                             Mmus   
> > 4 Q8BI84  <NA>      <NA>      Mia3      Melanoma inhibitory activity protein 3  Mmus   
> > 5 Q6P5D8  <NA>      <NA>      Smchd1    Structural maintenance of chromosomes … Mmus   
> > # … with abbreviated variable name ¹​organism
    
    {: .output} {: .solution} {: .challenge}

> ## Challenge
> 
> Join tables `jdf4` and `jdf5`, keeping observations observed in both tables.
> 
> > ## Solution
> > 
> > inner_join(jdf4, jdf5) 
    {: .language-r}
 Joining, by = "uniprot" 
    {: .output}
 # A tibble: 1 × 6 uniprot organelle entry     gene_name description         organism <chr>   <chr>     <chr>     <chr>     <chr>               <chr>   
> > 1 P19096  Cytosol   FAS_MOUSE Fasn      Fatty acid synthase Mmus    
> > {: .output} {: .solution} {: .challenge}

## Multiple matches

Sometimes, keys aren’t unique.

In the `jdf6` table below, we see that the accession number `Q99PL5` is repeated twice. According to this table, the ribosomial protein binding protein 1 localises in the endoplasmic reticulum and in the Golgi apparatus.

    jdf6

{: .language-r}

    # A tibble: 5 × 4
      uniprot organelle             entry       isoform
      <chr>   <chr>                 <chr>         <dbl>
    1 P26039  Actin cytoskeleton    TLN1_MOUSE        1
    2 Q99PL5  Endoplasmic reticulum RRBP1_MOUSE       1
    3 Q99PL5  Golgi apparatus       RRBP1_MOUSE       2
    4 Q6PB66  Mitochondrion         LPPRC_MOUSE       1
    5 P11276  Extracellular matrix  FINC_MOUSE        1

{: .output}

If we now want to join `jdf6` and `jdf2`, the variables of the latter will be duplicated.

    inner_join(jdf6, jdf2)

{: .language-r}

    Joining, by = "uniprot"

{: .output}

    # A tibble: 5 × 7
      uniprot organelle             entry       isoform gene_name description       organ…¹
      <chr>   <chr>                 <chr>         <dbl> <chr>     <chr>             <chr>  
    1 P26039  Actin cytoskeleton    TLN1_MOUSE        1 Tln1      Talin-1           Mmus   
    2 Q99PL5  Endoplasmic reticulum RRBP1_MOUSE       1 Rrbp1     Ribosome-binding… Mmus   
    3 Q99PL5  Golgi apparatus       RRBP1_MOUSE       2 Rrbp1     Ribosome-binding… Mmus   
    4 Q6PB66  Mitochondrion         LPPRC_MOUSE       1 Lrpprc    Leucine-rich PPR… Mmus   
    5 P11276  Extracellular matrix  FINC_MOUSE        1 Fn1       Fibronectin       Mmus   
    # … with abbreviated variable name ¹​organism

{: .output}

In the case above, repeating is useful, as it completes `jdf6` with correct information from `jdf2`.

But one needs however to be careful when duplicated keys exist in both tables.

Let’s now use jdf7 for the join. It also has 2 entries for the uniprot `Q99PL5`

    jdf7

{: .language-r}

    # A tibble: 5 × 6
      gene_name description                               uniprot organism isofor…¹ measure
      <chr>     <chr>                                     <chr>   <chr>       <dbl>   <dbl>
    1 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus            1     102
    2 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus            2       3
    3 Iqgap1    Ras GTPase-activating-like protein IQGAP1 Q9JKF1  Mmus            1      13
    4 Hspa5     78 kDa glucose-regulated protein          P20029  Mmus            1      54
    5 Pdcd11    Protein RRP5 homolog                      Q6NS46  Mmus            1      28
    # … with abbreviated variable name ¹​isoform_num

{: .output}

Let’s we create an inner join between `jdf6` and `jdf7` (both having duplicated `Q99PL5` entries)

    inner_join(jdf6, jdf7)

{: .language-r}

    Joining, by = "uniprot"

{: .output}

    # A tibble: 4 × 9
      uniprot organelle             entry   isoform gene_…¹ descr…² organ…³ isofo…⁴ measure
      <chr>   <chr>                 <chr>     <dbl> <chr>   <chr>   <chr>     <dbl>   <dbl>
    1 Q99PL5  Endoplasmic reticulum RRBP1_…       1 Rrbp1   Riboso… Mmus          1     102
    2 Q99PL5  Endoplasmic reticulum RRBP1_…       1 Rrbp1   Riboso… Mmus          2       3
    3 Q99PL5  Golgi apparatus       RRBP1_…       2 Rrbp1   Riboso… Mmus          1     102
    4 Q99PL5  Golgi apparatus       RRBP1_…       2 Rrbp1   Riboso… Mmus          2       3
    # … with abbreviated variable names ¹​gene_name, ²​description, ³​organism, ⁴​isoform_num

{: .output}

> ## Challenge
> 
> Interpret the result of the inner join above, where both tables have duplicated keys.
> 
> > ## Solution
> > 
> > `jdf6` has two entries, one for each possible sub-cellular localisation of the protein. `jdf7` has also two entries, referring to two different quantitative measurements (variable `measure`). When joining the duplicated keys, you get all possible combinations. <img src="../fig/join-many-to-many.png" alt="Joins with duplicated keys in both tables, producing all possible combinations. Figure taken from *R for Data Science*." width="70%" />
     <p class="caption">
> > Joins with duplicated keys in both tables, producing all possible
> > combinations. Figure taken from *R for Data Science*.
> > </p>
 In this case, we obtain wrong information: both proteins in the ER and in the GA both have value 102 and 3. inner_join(jdf6, jdf7) 
    {: .language-r}
 Joining, by = "uniprot" 
    {: .output}
 # A tibble: 4 × 9 uniprot organelle             entry   isoform gene_…¹ descr…² organ…³ isofo…⁴ measure <chr>   <chr>                 <chr>     <dbl> <chr>   <chr>   <chr>     <dbl>   <dbl> 1 Q99PL5  Endoplasmic reticulum RRBP1_…       1 Rrbp1   Riboso… Mmus          1     102 2 Q99PL5  Endoplasmic reticulum RRBP1_…       1 Rrbp1   Riboso… Mmus          2       3 3 Q99PL5  Golgi apparatus       RRBP1_…       2 Rrbp1   Riboso… Mmus          1     102 4 Q99PL5  Golgi apparatus       RRBP1_…       2 Rrbp1   Riboso… Mmus          2       3 # … with abbreviated variable names ¹​gene_name, ²​description, ³​organism, ⁴​isoform_num
    
    {: .output} {: .solution} {: .challenge}

## Matching across multiple keys

So far, we have matched tables using a single key (possibly with different names in the two tables). Sometimes, it is necessary to match tables using multiple keys. A typical example is when multiple variables are needed to discriminate different rows in a tables.

Following up from the last example, we see that the duplicated UniProt accession numbers in the `jdf6` and `jdf7` tables refer to different isoforms of the same RRBP1 gene.

    jdf6

{: .language-r}

    # A tibble: 5 × 4
      uniprot organelle             entry       isoform
      <chr>   <chr>                 <chr>         <dbl>
    1 P26039  Actin cytoskeleton    TLN1_MOUSE        1
    2 Q99PL5  Endoplasmic reticulum RRBP1_MOUSE       1
    3 Q99PL5  Golgi apparatus       RRBP1_MOUSE       2
    4 Q6PB66  Mitochondrion         LPPRC_MOUSE       1
    5 P11276  Extracellular matrix  FINC_MOUSE        1

{: .output}

    jdf7

{: .language-r}

    # A tibble: 5 × 6
      gene_name description                               uniprot organism isofor…¹ measure
      <chr>     <chr>                                     <chr>   <chr>       <dbl>   <dbl>
    1 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus            1     102
    2 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus            2       3
    3 Iqgap1    Ras GTPase-activating-like protein IQGAP1 Q9JKF1  Mmus            1      13
    4 Hspa5     78 kDa glucose-regulated protein          P20029  Mmus            1      54
    5 Pdcd11    Protein RRP5 homolog                      Q6NS46  Mmus            1      28
    # … with abbreviated variable name ¹​isoform_num

{: .output}

To uniquely identify isoforms, we should consider two keys:

-   the UniProt accession number (named `uniprot` in both tables)

-   and the isoform number (called `isoform` and `isoform_num` respectively)

Because the isoform status was encoded using different names (which is, of course a source of confusion), `jdf6` and `jdf7` are only automatically joined based on the shared `uniprot` key.

If the isoform status was encoded the same way in both tables, the join would have been automatically done on both keys!

Here, we need to join using both keys and need to explicitly name the variables used for the join.

    inner_join(jdf6, jdf7, by = c("uniprot" = "uniprot", "isoform" = "isoform_num"))

{: .language-r}

    # A tibble: 2 × 8
      uniprot organelle             entry       isoform gene_name descrip…¹ organ…² measure
      <chr>   <chr>                 <chr>         <dbl> <chr>     <chr>     <chr>     <dbl>
    1 Q99PL5  Endoplasmic reticulum RRBP1_MOUSE       1 Rrbp1     Ribosome… Mmus        102
    2 Q99PL5  Golgi apparatus       RRBP1_MOUSE       2 Rrbp1     Ribosome… Mmus          3
    # … with abbreviated variable names ¹​description, ²​organism

{: .output}

We now see that isoform 1 localised to the ER and has a measured value of 102, while isoform 2, that localised to the GA, has a measured value of 3.

Ideally, the isoform variables should be named identically in the two tables to enable an automatic join with the two keys.

An alternative could be to rename the `isoform_num` from jdf7 in order to have the both keys names present in both tables, enabling an automatic join. This can be done easily using the rename function from `dplyr` package.

    jdf7 %>% rename(isoform = isoform_num)

{: .language-r}

    # A tibble: 5 × 6
      gene_name description                               uniprot organism isoform measure
      <chr>     <chr>                                     <chr>   <chr>      <dbl>   <dbl>
    1 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus           1     102
    2 Rrbp1     Ribosome-binding protein 1                Q99PL5  Mmus           2       3
    3 Iqgap1    Ras GTPase-activating-like protein IQGAP1 Q9JKF1  Mmus           1      13
    4 Hspa5     78 kDa glucose-regulated protein          P20029  Mmus           1      54
    5 Pdcd11    Protein RRP5 homolog                      Q6NS46  Mmus           1      28

{: .output}

    inner_join(jdf6,
               jdf7 %>%
                 rename(isoform = isoform_num))

{: .language-r}

    Joining, by = c("uniprot", "isoform")

{: .output}

    # A tibble: 2 × 8
      uniprot organelle             entry       isoform gene_name descrip…¹ organ…² measure
      <chr>   <chr>                 <chr>         <dbl> <chr>     <chr>     <chr>     <dbl>
    1 Q99PL5  Endoplasmic reticulum RRBP1_MOUSE       1 Rrbp1     Ribosome… Mmus        102
    2 Q99PL5  Golgi apparatus       RRBP1_MOUSE       2 Rrbp1     Ribosome… Mmus          3
    # … with abbreviated variable names ¹​description, ²​organism

{: .output}

## Row and column binding

There are two other important functions in R, `rbind` and `cbind`, that can be used to combine two dataframes.

-   `cbind` can be used to bind two data frames by columns, but both must have the same number of rows.

<!-- -->

    d2

{: .language-r}

      a b
    1 4 4
    2 5 5

{: .output}

    d3

{: .language-r}

      v1 v2 v3
    1  1  3  5
    2  2  4  6

{: .output}

    cbind(d2, d3)

{: .language-r}

      a b v1 v2 v3
    1 4 4  1  3  5
    2 5 5  2  4  6

{: .output}

-   `rbind`, can be used to bind two data frames by rows, but both must have the same number of columns, and the same column names!

<!-- -->

    d1

{: .language-r}

      x y
    1 1 1
    2 2 2
    3 3 3

{: .output}

    d2

{: .language-r}

      a b
    1 4 4
    2 5 5

{: .output}

using `rbind(d1, d2)` would produce an error because both data frames do not have the same column names (even if they have the same number of columns)

If we change the names of d2, it works!

    names(d2) <- names(d1)
    d1

{: .language-r}

      x y
    1 1 1
    2 2 2
    3 3 3

{: .output}

    d2

{: .language-r}

      x y
    1 4 4
    2 5 5

{: .output}

    rbind(d1, d2)

{: .language-r}

      x y
    1 1 1
    2 2 2
    3 3 3
    4 4 4
    5 5 5

{: .output}

Note that beyond the dimensions and column names that are required to match, the real meaning of `rbind` is to bind data frames that contain observations for the same set of variables - there is more than only the column names!

**Note**: `rbind` and `cbind` are base R functions. The *tidyverse* alternatives from the `dplyr` package are `bind_rows` and `bind_cols` and work similarly.

[1] UniProt is the protein information database. Its mission is to *provide the scientific community with a comprehensive, high-quality and freely accessible resource of protein sequence and functional information*.
