## Data Manipulation using **`dplyr`** and **`tidyr`**

Bracket subsetting is handy, but it can be cumbersome and difficult to
read, especially for complicated operations.

Some packages can greatly facilitate our task when we manipulate data.
Packages in R are basically sets of additional functions that let you do
more stuff. The functions we’ve been using so far, like `str()` or
`data.frame()`, come built into R; Loading packages can give you access
to other specific functions. Before you use a package for the first time
you need to install it on your machine, and then you should import it in
every subsequent R session when you need it.

-   The package **`dplyr`** provides powerful tools for data
    manipulation tasks. It is built to work directly with data frames,
    with many manipulation tasks optimized.

-   As we will see latter on, sometimes we want a data frame to be
    reshaped to be able to do some specific analyses or for
    visualization. The package **`tidyr`** addresses this common problem
    of reshaping data and provides tools for manipulating data in a tidy
    way.

To learn more about **`dplyr`** and **`tidyr`** after the workshop, you
may want to check out this [handy data transformation with **`dplyr`**
cheatsheet](https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf)
and this [one about
**`tidyr`**](https://github.com/rstudio/cheatsheets/raw/master/data-import.pdf).

-   The **`tidyverse`** package is an “umbrella-package” that installs
    several useful packages for data analysis which work well together,
    such as **`tidyr`**, **`dplyr`**, **`ggplot2`**, **`tibble`**, etc.
    These packages help us to work and interact with the data. They
    allow us to do many things with your data, such as subsetting,
    transforming, visualizing, etc.

To install and load the **`tidyverse`** package type:

    BiocManager::install("tidyverse")

{: .language-r}

    ## load the tidyverse packages, incl. dplyr
    library("tidyverse")

{: .language-r}

## Loading data with tidyverse

Instead of `read.csv()`, we will read in our data using the `read_csv()`
function, from the tidyverse package **`readr`**, .

    rna <- read_csv("data/rnaseq.csv")

    ## view the data
    rna

{: .language-r}

    # A tibble: 32,428 × 19
       gene    sample     expre…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr>   <chr>        <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM2545336    1170 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  109900 argini… ENSMUS… 251000… 5      
     2 Apod    GSM2545336   36194 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   11815 apolip… ENSMUS… <NA>    16     
     3 Cyp2d22 GSM2545336    4060 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   56448 cytoch… ENSMUS… 2D22    15     
     4 Klk6    GSM2545336     287 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   19144 kallik… ENSMUS… Bssp    7      
     5 Fcrls   GSM2545336      85 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   80891 Fc rec… ENSMUS… 281043… 3      
     6 Slc2a4  GSM2545336     782 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   20528 solute… ENSMUS… Glut-4  11     
     7 Exd2    GSM2545336    1619 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   97827 exonuc… ENSMUS… 493053… 12     
     8 Gjc2    GSM2545336     288 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  118454 gap ju… ENSMUS… B23038… 11     
     9 Plp1    GSM2545336   43217 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   18823 proteo… ENSMUS… DM20    X      
    10 Gnb4    GSM2545336    1071 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   14696 guanin… ENSMUS… 672045… 3      
    # … with 32,418 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

Notice that the class of the data is now referred to as a “tibble”.

Tibbles tweak some of the behaviors of the data frame objects we
introduced in the previously. The data structure is very similar to a
data frame. For our purposes the only differences are that:

1.  It displays the data type of each column under its name. Note that
    &lt;`dbl`&gt; is a data type defined to hold numeric values with
    decimal points.

2.  It only prints the first few rows of data and only as many columns
    as fit on one screen.

We are now going to learn some of the most common **`dplyr`** functions:

-   `select()`: subset columns
-   `filter()`: subset rows on conditions
-   `mutate()`: create new columns by using information from other
    columns
-   `group_by()` and `summarize()`: create summary statistics on grouped
    data
-   `arrange()`: sort results
-   `count()`: count discrete values

## Selecting columns and filtering rows

To select columns of a data frame, use `select()`. The first argument to
this function is the data frame (`rna`), and the subsequent arguments
are the columns to keep.

    select(rna, gene, sample, tissue, expression)

{: .language-r}

    # A tibble: 32,428 × 4
       gene    sample     tissue     expression
       <chr>   <chr>      <chr>           <dbl>
     1 Asl     GSM2545336 Cerebellum       1170
     2 Apod    GSM2545336 Cerebellum      36194
     3 Cyp2d22 GSM2545336 Cerebellum       4060
     4 Klk6    GSM2545336 Cerebellum        287
     5 Fcrls   GSM2545336 Cerebellum         85
     6 Slc2a4  GSM2545336 Cerebellum        782
     7 Exd2    GSM2545336 Cerebellum       1619
     8 Gjc2    GSM2545336 Cerebellum        288
     9 Plp1    GSM2545336 Cerebellum      43217
    10 Gnb4    GSM2545336 Cerebellum       1071
    # … with 32,418 more rows

{: .output}

To select all columns *except* certain ones, put a “-” in front of the
variable to exclude it.

    select(rna, -tissue, -organism)

{: .language-r}

    # A tibble: 32,428 × 17
       gene    sample    expre…¹   age sex   infec…² strain  time mouse ENTRE…³ product ensem…⁴ exter…⁵ chrom…⁶ gene_…⁷ pheno…⁸
       <chr>   <chr>       <dbl> <dbl> <chr> <chr>   <chr>  <dbl> <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM25453…    1170     8 Fema… Influe… C57BL…     8    14  109900 argini… ENSMUS… 251000… 5       protei… abnorm…
     2 Apod    GSM25453…   36194     8 Fema… Influe… C57BL…     8    14   11815 apolip… ENSMUS… <NA>    16      protei… abnorm…
     3 Cyp2d22 GSM25453…    4060     8 Fema… Influe… C57BL…     8    14   56448 cytoch… ENSMUS… 2D22    15      protei… abnorm…
     4 Klk6    GSM25453…     287     8 Fema… Influe… C57BL…     8    14   19144 kallik… ENSMUS… Bssp    7       protei… abnorm…
     5 Fcrls   GSM25453…      85     8 Fema… Influe… C57BL…     8    14   80891 Fc rec… ENSMUS… 281043… 3       protei… decrea…
     6 Slc2a4  GSM25453…     782     8 Fema… Influe… C57BL…     8    14   20528 solute… ENSMUS… Glut-4  11      protei… abnorm…
     7 Exd2    GSM25453…    1619     8 Fema… Influe… C57BL…     8    14   97827 exonuc… ENSMUS… 493053… 12      protei… <NA>   
     8 Gjc2    GSM25453…     288     8 Fema… Influe… C57BL…     8    14  118454 gap ju… ENSMUS… B23038… 11      protei… Purkin…
     9 Plp1    GSM25453…   43217     8 Fema… Influe… C57BL…     8    14   18823 proteo… ENSMUS… DM20    X       protei… abnorm…
    10 Gnb4    GSM25453…    1071     8 Fema… Influe… C57BL…     8    14   14696 guanin… ENSMUS… 672045… 3       protei… decrea…
    # … with 32,418 more rows, 1 more variable: hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names
    #   ¹​expression, ²​infection, ³​ENTREZID, ⁴​ensembl_gene_id, ⁵​external_synonym, ⁶​chromosome_name, ⁷​gene_biotype,
    #   ⁸​phenotype_description

{: .output}

This will select all the variables in `rna` except `tissue` and
`organism`.

To choose rows based on a specific criteria, use `filter()`:

    filter(rna, sex == "Male")

{: .language-r}

    # A tibble: 14,740 × 19
       gene    sample     expre…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr>   <chr>        <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM2545340     626 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18  109900 argini… ENSMUS… 251000… 5      
     2 Apod    GSM2545340   13021 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   11815 apolip… ENSMUS… <NA>    16     
     3 Cyp2d22 GSM2545340    2171 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   56448 cytoch… ENSMUS… 2D22    15     
     4 Klk6    GSM2545340     448 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   19144 kallik… ENSMUS… Bssp    7      
     5 Fcrls   GSM2545340     180 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   80891 Fc rec… ENSMUS… 281043… 3      
     6 Slc2a4  GSM2545340     313 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   20528 solute… ENSMUS… Glut-4  11     
     7 Exd2    GSM2545340    2366 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   97827 exonuc… ENSMUS… 493053… 12     
     8 Gjc2    GSM2545340     310 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18  118454 gap ju… ENSMUS… B23038… 11     
     9 Plp1    GSM2545340   53126 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   18823 proteo… ENSMUS… DM20    X      
    10 Gnb4    GSM2545340    1355 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18   14696 guanin… ENSMUS… 672045… 3      
    # … with 14,730 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

    filter(rna, sex == "Male" & infection == "NonInfected")

{: .language-r}

    # A tibble: 4,422 × 19
       gene    sample     expre…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr>   <chr>        <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM2545343     535 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11  109900 argini… ENSMUS… 251000… 5      
     2 Apod    GSM2545343   13668 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   11815 apolip… ENSMUS… <NA>    16     
     3 Cyp2d22 GSM2545343    2008 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   56448 cytoch… ENSMUS… 2D22    15     
     4 Klk6    GSM2545343    1101 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   19144 kallik… ENSMUS… Bssp    7      
     5 Fcrls   GSM2545343     375 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   80891 Fc rec… ENSMUS… 281043… 3      
     6 Slc2a4  GSM2545343     249 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   20528 solute… ENSMUS… Glut-4  11     
     7 Exd2    GSM2545343    3126 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   97827 exonuc… ENSMUS… 493053… 12     
     8 Gjc2    GSM2545343     791 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11  118454 gap ju… ENSMUS… B23038… 11     
     9 Plp1    GSM2545343   98658 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   18823 proteo… ENSMUS… DM20    X      
    10 Gnb4    GSM2545343    2437 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11   14696 guanin… ENSMUS… 672045… 3      
    # … with 4,412 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

Now let’s imagine we are interested in the human homologs of the mouse
genes analysed in this dataset. This information can be found in the
last column of the `rna` tibble, named
`hsapiens_homolog_associated_gene_name`. Some mouse gene have no human
homologs. These can be retrieved using a `filter()` in the chain, and
the `is.na()` function that determines whether something is an `NA`.

    rna_NA <- filter(rna, is.na(hsapiens_homolog_associated_gene_name))
    select(rna_NA, gene, hsapiens_homolog_associated_gene_name)

{: .language-r}

    # A tibble: 4,290 × 2
       gene     hsapiens_homolog_associated_gene_name
       <chr>    <chr>                                
     1 Prodh    <NA>                                 
     2 Tssk5    <NA>                                 
     3 Vmn2r1   <NA>                                 
     4 Gm10654  <NA>                                 
     5 Hexa     <NA>                                 
     6 Sult1a1  <NA>                                 
     7 Gm6277   <NA>                                 
     8 Tmem198b <NA>                                 
     9 Adam1a   <NA>                                 
    10 Ebp      <NA>                                 
    # … with 4,280 more rows

{: .output}

If we want to keep only mouse gene that have a human homolog, we can
insert a “!” symbol that negates the result, so we’re asking for every
row where hsapiens\_homolog\_associated\_gene\_name *is not* an `NA`.

    rna_no_NA <- filter(rna, !is.na(hsapiens_homolog_associated_gene_name))
    select(rna_no_NA, gene, hsapiens_homolog_associated_gene_name)

{: .language-r}

    # A tibble: 28,138 × 2
       gene    hsapiens_homolog_associated_gene_name
       <chr>   <chr>                                
     1 Asl     ASL                                  
     2 Apod    APOD                                 
     3 Cyp2d22 CYP2D6                               
     4 Klk6    KLK6                                 
     5 Fcrls   FCRL2                                
     6 Slc2a4  SLC2A4                               
     7 Exd2    EXD2                                 
     8 Gjc2    GJC2                                 
     9 Plp1    PLP1                                 
    10 Gnb4    GNB4                                 
    # … with 28,128 more rows

{: .output}

## Pipes

What if you want to select and filter at the same time? There are three
ways to do this: use intermediate steps, nested functions, or pipes.

With intermediate steps, you create a temporary data frame and use that
as input to the next function, like this:

    rna2 <- filter(rna, sex == "Male")
    rna3 <- select(rna2, gene, sample, tissue, expression)
    rna3

{: .language-r}

    # A tibble: 14,740 × 4
       gene    sample     tissue     expression
       <chr>   <chr>      <chr>           <dbl>
     1 Asl     GSM2545340 Cerebellum        626
     2 Apod    GSM2545340 Cerebellum      13021
     3 Cyp2d22 GSM2545340 Cerebellum       2171
     4 Klk6    GSM2545340 Cerebellum        448
     5 Fcrls   GSM2545340 Cerebellum        180
     6 Slc2a4  GSM2545340 Cerebellum        313
     7 Exd2    GSM2545340 Cerebellum       2366
     8 Gjc2    GSM2545340 Cerebellum        310
     9 Plp1    GSM2545340 Cerebellum      53126
    10 Gnb4    GSM2545340 Cerebellum       1355
    # … with 14,730 more rows

{: .output}

This is readable, but can clutter up your workspace with lots of
intermediate objects that you have to name individually. With multiple
steps, that can be hard to keep track of.

You can also nest functions (i.e. one function inside of another), like
this:

    rna3 <- select(filter(rna, sex == "Male"), gene, sample, tissue, expression)
    rna3

{: .language-r}

    # A tibble: 14,740 × 4
       gene    sample     tissue     expression
       <chr>   <chr>      <chr>           <dbl>
     1 Asl     GSM2545340 Cerebellum        626
     2 Apod    GSM2545340 Cerebellum      13021
     3 Cyp2d22 GSM2545340 Cerebellum       2171
     4 Klk6    GSM2545340 Cerebellum        448
     5 Fcrls   GSM2545340 Cerebellum        180
     6 Slc2a4  GSM2545340 Cerebellum        313
     7 Exd2    GSM2545340 Cerebellum       2366
     8 Gjc2    GSM2545340 Cerebellum        310
     9 Plp1    GSM2545340 Cerebellum      53126
    10 Gnb4    GSM2545340 Cerebellum       1355
    # … with 14,730 more rows

{: .output}

This is handy, but can be difficult to read if too many functions are
nested, as R evaluates the expression from the inside out (in this case,
filtering, then selecting).

The last option, *pipes*, are a recent addition to R. Pipes let you take
the output of one function and send it directly to the next, which is
useful when you need to do many things to the same dataset.

Pipes in R look like `%>%` and are made available via the **`magrittr`**
package, installed automatically with **`dplyr`**. If you use RStudio,
you can type the pipe with <kbd>Ctrl</kbd> + <kbd>Shift</kbd> +
<kbd>M</kbd> if you have a PC or <kbd>Cmd</kbd> + <kbd>Shift</kbd> +
<kbd>M</kbd> if you have a Mac.

In the above code, we use the pipe to send the `rna` dataset first
through `filter()` to keep rows where `sex` is Male, then through
`select()` to keep only the `gene`, `sample`, `tissue`, and
`expression`columns.

The pipe `%>%` takes the object on its left and passes it directly as
the first argument to the function on its right, we don’t need to
explicitly include the data frame as an argument to the `filter()` and
`select()` functions any more.

    rna %>%
      filter(sex == "Male") %>%
      select(gene, sample, tissue, expression)

{: .language-r}

    # A tibble: 14,740 × 4
       gene    sample     tissue     expression
       <chr>   <chr>      <chr>           <dbl>
     1 Asl     GSM2545340 Cerebellum        626
     2 Apod    GSM2545340 Cerebellum      13021
     3 Cyp2d22 GSM2545340 Cerebellum       2171
     4 Klk6    GSM2545340 Cerebellum        448
     5 Fcrls   GSM2545340 Cerebellum        180
     6 Slc2a4  GSM2545340 Cerebellum        313
     7 Exd2    GSM2545340 Cerebellum       2366
     8 Gjc2    GSM2545340 Cerebellum        310
     9 Plp1    GSM2545340 Cerebellum      53126
    10 Gnb4    GSM2545340 Cerebellum       1355
    # … with 14,730 more rows

{: .output}

Some may find it helpful to read the pipe like the word “then”. For
instance, in the above example, we took the data frame `rna`, *then* we
`filter`ed for rows with `sex == "Male"`, *then* we `select`ed columns
`gene`, `sample`, `tissue`, and `expression`.

The **`dplyr`** functions by themselves are somewhat simple, but by
combining them into linear workflows with the pipe, we can accomplish
more complex manipulations of data frames.

If we want to create a new object with this smaller version of the data,
we can assign it a new name:

    rna3 <- rna %>%
      filter(sex == "Male") %>%
      select(gene, sample, tissue, expression)

    rna3

{: .language-r}

    # A tibble: 14,740 × 4
       gene    sample     tissue     expression
       <chr>   <chr>      <chr>           <dbl>
     1 Asl     GSM2545340 Cerebellum        626
     2 Apod    GSM2545340 Cerebellum      13021
     3 Cyp2d22 GSM2545340 Cerebellum       2171
     4 Klk6    GSM2545340 Cerebellum        448
     5 Fcrls   GSM2545340 Cerebellum        180
     6 Slc2a4  GSM2545340 Cerebellum        313
     7 Exd2    GSM2545340 Cerebellum       2366
     8 Gjc2    GSM2545340 Cerebellum        310
     9 Plp1    GSM2545340 Cerebellum      53126
    10 Gnb4    GSM2545340 Cerebellum       1355
    # … with 14,730 more rows

{: .output}

> ## Challenge:
>
> Using pipes, subset the `rna` data to keep genes with an expression
> higher than 50000 in female mice at time 0, and retain only the
> columns `gene`, `sample`, `time`, `expression` and `age`
>
> > ## Solution
> >
> >     rna %>%
> >       filter(expression > 50000,
> >              sex == "Female",
> >              time == 0 ) %>%
> >       select(gene, sample, time, expression, age)
> >
> > {: .language-r}
> >
> >     # A tibble: 9 × 5
> >       gene   sample      time expression   age
> >       <chr>  <chr>      <dbl>      <dbl> <dbl>
> >     1 Plp1   GSM2545337     0     101241     8
> >     2 Atp1b1 GSM2545337     0      53260     8
> >     3 Plp1   GSM2545338     0      96534     8
> >     4 Atp1b1 GSM2545338     0      50614     8
> >     5 Plp1   GSM2545348     0     102790     8
> >     6 Atp1b1 GSM2545348     0      59544     8
> >     7 Plp1   GSM2545353     0      71237     8
> >     8 Glul   GSM2545353     0      52451     8
> >     9 Atp1b1 GSM2545353     0      61451     8
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

## Mutate

Frequently you’ll want to create new columns based on the values of
existing columns, for example to do unit conversions, or to find the
ratio of values in two columns. For this we’ll use `mutate()`.

To create a new column of time in hours:

    rna %>%
      mutate(time_hours = time * 24) %>%
      select(time, time_hours)

{: .language-r}

    # A tibble: 32,428 × 2
        time time_hours
       <dbl>      <dbl>
     1     8        192
     2     8        192
     3     8        192
     4     8        192
     5     8        192
     6     8        192
     7     8        192
     8     8        192
     9     8        192
    10     8        192
    # … with 32,418 more rows

{: .output}

You can also create a second new column based on the first new column
within the same call of `mutate()`:

    rna %>%
      mutate(time_hours = time * 24,
             time_mn = time_hours * 60) %>%
      select(time, time_hours, time_mn)

{: .language-r}

    # A tibble: 32,428 × 3
        time time_hours time_mn
       <dbl>      <dbl>   <dbl>
     1     8        192   11520
     2     8        192   11520
     3     8        192   11520
     4     8        192   11520
     5     8        192   11520
     6     8        192   11520
     7     8        192   11520
     8     8        192   11520
     9     8        192   11520
    10     8        192   11520
    # … with 32,418 more rows

{: .output}

> ## Challenge
>
> Create a new data frame from the `rna` data that meets the following
> criteria: contains only the `gene`, `chromosome_name`,
> `phenotype_description`, `sample`, and `expression` columns and a new
> column giving the log expression the gene. This data frame must only
> contain genes located on autosomes and associated with a
> phenotype\_description.
>
> **Hint**: think about how the commands should be ordered to produce
> this data frame!
>
> > ## Solution
> >
> >     rna %>%
> >       filter(chromosome_name != "X", chromosome_name != "Y") %>%
> >       mutate(log_expression = log(expression)) %>%
> >       select(gene, chromosome_name, phenotype_description, sample, log_expression) %>%
> >       filter(!is.na(phenotype_description))
> >
> > {: .language-r}
> >
> >     # A tibble: 21,054 × 5
> >        gene    chromosome_name phenotype_description                           sample     log_expression
> >        <chr>   <chr>           <chr>                                           <chr>               <dbl>
> >      1 Asl     5               abnormal circulating amino acid level           GSM2545336           7.06
> >      2 Apod    16              abnormal lipid homeostasis                      GSM2545336          10.5 
> >      3 Cyp2d22 15              abnormal skin morphology                        GSM2545336           8.31
> >      4 Klk6    7               abnormal cytokine level                         GSM2545336           5.66
> >      5 Fcrls   3               decreased CD8-positive alpha-beta T cell number GSM2545336           4.44
> >      6 Slc2a4  11              abnormal circulating glucose level              GSM2545336           6.66
> >      7 Gjc2    11              Purkinje cell degeneration                      GSM2545336           5.66
> >      8 Gnb4    3               decreased anxiety-related response              GSM2545336           6.98
> >      9 Tnc     4               abnormal CNS synaptic transmission              GSM2545336           5.39
> >     10 Trf     9               abnormal circulating phosphate level            GSM2545336           9.18
> >     # … with 21,044 more rows
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

## Split-apply-combine data analysis

Many data analysis tasks can be approached using the
*split-apply-combine* paradigm: split the data into groups, apply some
analysis to each group, and then combine the results. **`dplyr`** makes
this very easy through the use of the `group_by()` function.

    rna %>%
      group_by(gene)

{: .language-r}

    # A tibble: 32,428 × 19
    # Groups:   gene [1,474]
       gene    sample     expre…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr>   <chr>        <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM2545336    1170 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  109900 argini… ENSMUS… 251000… 5      
     2 Apod    GSM2545336   36194 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   11815 apolip… ENSMUS… <NA>    16     
     3 Cyp2d22 GSM2545336    4060 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   56448 cytoch… ENSMUS… 2D22    15     
     4 Klk6    GSM2545336     287 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   19144 kallik… ENSMUS… Bssp    7      
     5 Fcrls   GSM2545336      85 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   80891 Fc rec… ENSMUS… 281043… 3      
     6 Slc2a4  GSM2545336     782 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   20528 solute… ENSMUS… Glut-4  11     
     7 Exd2    GSM2545336    1619 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   97827 exonuc… ENSMUS… 493053… 12     
     8 Gjc2    GSM2545336     288 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  118454 gap ju… ENSMUS… B23038… 11     
     9 Plp1    GSM2545336   43217 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   18823 proteo… ENSMUS… DM20    X      
    10 Gnb4    GSM2545336    1071 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   14696 guanin… ENSMUS… 672045… 3      
    # … with 32,418 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

The `group_by()` function doesn’t perform any data processing, it groups
the data into subsets: in the example above, our initial `tibble` of
32428 observations is split into 1474 groups based on the `gene`
variable.

We could similarly decide to group the tibble by the samples:

    rna %>%
      group_by(sample)

{: .language-r}

    # A tibble: 32,428 × 19
    # Groups:   sample [22]
       gene    sample     expre…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr>   <chr>        <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Asl     GSM2545336    1170 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  109900 argini… ENSMUS… 251000… 5      
     2 Apod    GSM2545336   36194 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   11815 apolip… ENSMUS… <NA>    16     
     3 Cyp2d22 GSM2545336    4060 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   56448 cytoch… ENSMUS… 2D22    15     
     4 Klk6    GSM2545336     287 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   19144 kallik… ENSMUS… Bssp    7      
     5 Fcrls   GSM2545336      85 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   80891 Fc rec… ENSMUS… 281043… 3      
     6 Slc2a4  GSM2545336     782 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   20528 solute… ENSMUS… Glut-4  11     
     7 Exd2    GSM2545336    1619 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   97827 exonuc… ENSMUS… 493053… 12     
     8 Gjc2    GSM2545336     288 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  118454 gap ju… ENSMUS… B23038… 11     
     9 Plp1    GSM2545336   43217 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   18823 proteo… ENSMUS… DM20    X      
    10 Gnb4    GSM2545336    1071 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14   14696 guanin… ENSMUS… 672045… 3      
    # … with 32,418 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

Here our initial `tibble` of 32428 observations is split into 22 groups
based on the `sample` variable.

Once the data have been combined, subsequent operations will be applied
on each group independently.

### The `summarize()` function

`group_by()` is often used together with `summarize()`, which collapses
each group into a single-row summary of that group.

`group_by()` takes as arguments the column names that contain the
**categorical** variables for which you want to calculate the summary
statistics. So to compute the mean `expression` by gene:

    rna %>%
      group_by(gene) %>%
      summarize(mean_expression = mean(expression))

{: .language-r}

    # A tibble: 1,474 × 2
       gene    mean_expression
       <chr>             <dbl>
     1 Aamp            4751.  
     2 Abca12             4.55
     3 Abcc8           2498.  
     4 Abhd14a          525.  
     5 Abi2            4909.  
     6 Abi3bp          1002.  
     7 Abl2            2124.  
     8 Acadl           2053.  
     9 Acap3           3536.  
    10 Acbd4           1431.  
    # … with 1,464 more rows

{: .output}

We could also want to calculate the mean expression levels of all genes
in each sample:

    rna %>%
      group_by(sample) %>%
      summarize(mean_expression = mean(expression))

{: .language-r}

    # A tibble: 22 × 2
       sample     mean_expression
       <chr>                <dbl>
     1 GSM2545336           2062.
     2 GSM2545337           1766.
     3 GSM2545338           1668.
     4 GSM2545339           1696.
     5 GSM2545340           1682.
     6 GSM2545341           1638.
     7 GSM2545342           1594.
     8 GSM2545343           2107.
     9 GSM2545344           1712.
    10 GSM2545345           1700.
    # … with 12 more rows

{: .output}

But we can can also group by multiple columns:

    rna %>%
      group_by(gene, infection, time) %>%
      summarize(mean_expression = mean(expression))

{: .language-r}

    `summarise()` has grouped output by 'gene', 'infection'. You can override using the `.groups` argument.

{: .output}

    # A tibble: 4,422 × 4
    # Groups:   gene, infection [2,948]
       gene    infection    time mean_expression
       <chr>   <chr>       <dbl>           <dbl>
     1 Aamp    InfluenzaA      4         4870   
     2 Aamp    InfluenzaA      8         4763.  
     3 Aamp    NonInfected     0         4603.  
     4 Abca12  InfluenzaA      4            4.25
     5 Abca12  InfluenzaA      8            4.14
     6 Abca12  NonInfected     0            5.29
     7 Abcc8   InfluenzaA      4         2609.  
     8 Abcc8   InfluenzaA      8         2292.  
     9 Abcc8   NonInfected     0         2576.  
    10 Abhd14a InfluenzaA      4          547.  
    # … with 4,412 more rows

{: .output}

Once the data is grouped, you can also summarize multiple variables at
the same time (and not necessarily on the same variable). For instance,
we could add a column indicating the median `expression` by gene and by
condition:

    rna %>%
      group_by(gene, infection, time) %>%
      summarize(mean_expression = mean(expression),
                median_expression = median(expression))

{: .language-r}

    `summarise()` has grouped output by 'gene', 'infection'. You can override using the `.groups` argument.

{: .output}

    # A tibble: 4,422 × 5
    # Groups:   gene, infection [2,948]
       gene    infection    time mean_expression median_expression
       <chr>   <chr>       <dbl>           <dbl>             <dbl>
     1 Aamp    InfluenzaA      4         4870               4708  
     2 Aamp    InfluenzaA      8         4763.              4813  
     3 Aamp    NonInfected     0         4603.              4717  
     4 Abca12  InfluenzaA      4            4.25               4.5
     5 Abca12  InfluenzaA      8            4.14               4  
     6 Abca12  NonInfected     0            5.29               5  
     7 Abcc8   InfluenzaA      4         2609.              2424. 
     8 Abcc8   InfluenzaA      8         2292.              2224  
     9 Abcc8   NonInfected     0         2576.              2578  
    10 Abhd14a InfluenzaA      4          547.               523  
    # … with 4,412 more rows

{: .output}

> ## Challenge
>
> Calculate the mean expression level of gene “Dok3” by timepoints.
>
> > ## Solution
> >
> >     rna %>%
> >       filter(gene == "Dok3") %>%
> >       group_by(time) %>%
> >       summarize(mean = mean(expression))
> >
> > {: .language-r}
> >
> >     # A tibble: 3 × 2
> >        time  mean
> >       <dbl> <dbl>
> >     1     0  169 
> >     2     4  156.
> >     3     8   61 
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

### Counting

When working with data, we often want to know the number of observations
found for each factor or combination of factors. For this task,
**`dplyr`** provides `count()`. For example, if we wanted to count the
number of rows of data for each infected and non infected samples, we
would do:

    rna %>%
        count(infection)

{: .language-r}

    # A tibble: 2 × 2
      infection       n
      <chr>       <int>
    1 InfluenzaA  22110
    2 NonInfected 10318

{: .output}

The `count()` function is shorthand for something we’ve already seen:
grouping by a variable, and summarizing it by counting the number of
observations in that group. In other words, `rna %>% count()` is
equivalent to:

    rna %>%
        group_by(infection) %>%
        summarise(n = n())

{: .language-r}

    # A tibble: 2 × 2
      infection       n
      <chr>       <int>
    1 InfluenzaA  22110
    2 NonInfected 10318

{: .output}

Previous example shows the use of `count()` to count the number of
rows/observations for *one* factor (i.e., `infection`). If we wanted to
count *combination of factors*, such as `infection` and `time`, we would
specify the first and the second factor as the arguments of `count()`:

    rna %>%
        count(infection, time)

{: .language-r}

    # A tibble: 3 × 3
      infection    time     n
      <chr>       <dbl> <int>
    1 InfluenzaA      4 11792
    2 InfluenzaA      8 10318
    3 NonInfected     0 10318

{: .output}

which is equivalent to this:

    rna %>%
      group_by(infection, time) %>%
      summarize(n = n())

{: .language-r}

    `summarise()` has grouped output by 'infection'. You can override using the `.groups` argument.

{: .output}

    # A tibble: 3 × 3
    # Groups:   infection [2]
      infection    time     n
      <chr>       <dbl> <int>
    1 InfluenzaA      4 11792
    2 InfluenzaA      8 10318
    3 NonInfected     0 10318

{: .output}

It is sometimes useful to sort the result to facilitate the comparisons.
We can use `arrange()` to sort the table. For instance, we might want to
arrange the table above by time:

    rna %>%
      count(infection, time) %>%
      arrange(time)

{: .language-r}

    # A tibble: 3 × 3
      infection    time     n
      <chr>       <dbl> <int>
    1 NonInfected     0 10318
    2 InfluenzaA      4 11792
    3 InfluenzaA      8 10318

{: .output}

or by counts:

    rna %>%
      count(infection, time) %>%
      arrange(n)

{: .language-r}

    # A tibble: 3 × 3
      infection    time     n
      <chr>       <dbl> <int>
    1 InfluenzaA      8 10318
    2 NonInfected     0 10318
    3 InfluenzaA      4 11792

{: .output}

To sort in descending order, we need to add the `desc()` function:

    rna %>%
      count(infection, time) %>%
      arrange(desc(n))

{: .language-r}

    # A tibble: 3 × 3
      infection    time     n
      <chr>       <dbl> <int>
    1 InfluenzaA      4 11792
    2 InfluenzaA      8 10318
    3 NonInfected     0 10318

{: .output}

> ## Challenge
>
> 1.  How many genes were analysed in each sample?
> 2.  Use `group_by()` and `summarize()` to evaluate the sequencing
>     depth (the sum of all counts) in each sample. Which sample has the
>     highest sequencing depth?
> 3.  Pick one sample and evaluate the number of genes by biotype
> 4.  Identify genes associated with “abnormal DNA methylation”
>     phenotype description, and calculate their mean expression (in
>     log) at time 0, time 4 and time 8.
>
> > ## Solution
> >
> >     ## 1.
> >     rna %>%
> >       count(sample)
> >
> > {: .language-r}
> >
> >     # A tibble: 22 × 2
> >        sample         n
> >        <chr>      <int>
> >      1 GSM2545336  1474
> >      2 GSM2545337  1474
> >      3 GSM2545338  1474
> >      4 GSM2545339  1474
> >      5 GSM2545340  1474
> >      6 GSM2545341  1474
> >      7 GSM2545342  1474
> >      8 GSM2545343  1474
> >      9 GSM2545344  1474
> >     10 GSM2545345  1474
> >     # … with 12 more rows
> >
> > {: .output}
> >
> >     ## 2.
> >     rna %>%
> >       group_by(sample) %>%
> >       summarize(seq_depth = sum(expression)) %>%
> >       arrange(desc(seq_depth))
> >
> > {: .language-r}
> >
> >     # A tibble: 22 × 2
> >        sample     seq_depth
> >        <chr>          <dbl>
> >      1 GSM2545350   3255566
> >      2 GSM2545352   3216163
> >      3 GSM2545343   3105652
> >      4 GSM2545336   3039671
> >      5 GSM2545380   3036098
> >      6 GSM2545353   2953249
> >      7 GSM2545348   2913678
> >      8 GSM2545362   2913517
> >      9 GSM2545351   2782464
> >     10 GSM2545349   2758006
> >     # … with 12 more rows
> >
> > {: .output}
> >
> >     ## 3.
> >     rna %>%
> >       filter(sample == "GSM2545336") %>%
> >       group_by(gene_biotype) %>%
> >       count(gene_biotype) %>%
> >       arrange(desc(n))
> >
> > {: .language-r}
> >
> >     # A tibble: 13 × 2
> >     # Groups:   gene_biotype [13]
> >        gene_biotype                           n
> >        <chr>                              <int>
> >      1 protein_coding                      1321
> >      2 lncRNA                                69
> >      3 processed_pseudogene                  59
> >      4 miRNA                                  7
> >      5 snoRNA                                 5
> >      6 TEC                                    4
> >      7 polymorphic_pseudogene                 2
> >      8 unprocessed_pseudogene                 2
> >      9 IG_C_gene                              1
> >     10 scaRNA                                 1
> >     11 transcribed_processed_pseudogene       1
> >     12 transcribed_unitary_pseudogene         1
> >     13 transcribed_unprocessed_pseudogene     1
> >
> > {: .output}
> >
> >     ## 4.
> >     rna %>%
> >       filter(phenotype_description == "abnormal DNA methylation") %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_expression = mean(log(expression))) %>%
> >       arrange()
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     # A tibble: 6 × 3
> >     # Groups:   gene [2]
> >       gene   time mean_expression
> >       <chr> <dbl>           <dbl>
> >     1 Xist      0            6.95
> >     2 Xist      4            6.34
> >     3 Xist      8            7.13
> >     4 Zdbf2     0            6.27
> >     5 Zdbf2     4            6.27
> >     6 Zdbf2     8            6.19
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

## Reshaping data

In the `rna` tibble, the rows contain expression values (the unit) that
are associated with a combination of 2 other variables: `gene` and
`sample`.

All the other columns correspond to variables describing either the
sample (organism, age, sex,…) or the gene (gene\_biotype, ENTREZ\_ID,
product…). The variables that don’t change with genes or with samples
will have the same value in all the rows.

    rna %>%
      arrange(gene)

{: .language-r}

    # A tibble: 32,428 × 19
       gene  sample     express…¹ organ…²   age sex   infec…³ strain  time tissue mouse ENTRE…⁴ product ensem…⁵ exter…⁶ chrom…⁷
       <chr> <chr>          <dbl> <chr>   <dbl> <chr> <chr>   <chr>  <dbl> <chr>  <dbl>   <dbl> <chr>   <chr>   <chr>   <chr>  
     1 Aamp  GSM2545336      5621 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…    14  227290 angio-… ENSMUS… Aamp-rs 1      
     2 Aamp  GSM2545337      4049 Mus mu…     8 Fema… NonInf… C57BL…     0 Cereb…     9  227290 angio-… ENSMUS… Aamp-rs 1      
     3 Aamp  GSM2545338      3797 Mus mu…     8 Fema… NonInf… C57BL…     0 Cereb…    10  227290 angio-… ENSMUS… Aamp-rs 1      
     4 Aamp  GSM2545339      4375 Mus mu…     8 Fema… Influe… C57BL…     4 Cereb…    15  227290 angio-… ENSMUS… Aamp-rs 1      
     5 Aamp  GSM2545340      4095 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    18  227290 angio-… ENSMUS… Aamp-rs 1      
     6 Aamp  GSM2545341      3867 Mus mu…     8 Male  Influe… C57BL…     8 Cereb…     6  227290 angio-… ENSMUS… Aamp-rs 1      
     7 Aamp  GSM2545342      3578 Mus mu…     8 Fema… Influe… C57BL…     8 Cereb…     5  227290 angio-… ENSMUS… Aamp-rs 1      
     8 Aamp  GSM2545343      5097 Mus mu…     8 Male  NonInf… C57BL…     0 Cereb…    11  227290 angio-… ENSMUS… Aamp-rs 1      
     9 Aamp  GSM2545344      4202 Mus mu…     8 Fema… Influe… C57BL…     4 Cereb…    22  227290 angio-… ENSMUS… Aamp-rs 1      
    10 Aamp  GSM2545345      4701 Mus mu…     8 Male  Influe… C57BL…     4 Cereb…    13  227290 angio-… ENSMUS… Aamp-rs 1      
    # … with 32,418 more rows, 3 more variables: gene_biotype <chr>, phenotype_description <chr>,
    #   hsapiens_homolog_associated_gene_name <chr>, and abbreviated variable names ¹​expression, ²​organism, ³​infection,
    #   ⁴​ENTREZID, ⁵​ensembl_gene_id, ⁶​external_synonym, ⁷​chromosome_name

{: .output}

This structure is called a `long-format`, as one column contains all the
values, and other column(s) list(s) the context of the value.

In certain cases, the `long-format` is not really “human-readable”, and
another format, a `wide-format` is preferred, as a more compact way of
representing the data. This is typically the case with gene expression
values that scientists are used to look as matrices, were rows represent
genes and columns represent samples.

In this format, it would become therefore straightforward to explore the
relationship between the gene expression levels within, and between, the
samples.

    # A tibble: 1,474 × 23
       gene    GSM254…¹ GSM25…² GSM25…³ GSM25…⁴ GSM25…⁵ GSM25…⁶ GSM25…⁷ GSM25…⁸ GSM25…⁹ GSM25…˟ GSM25…˟ GSM25…˟ GSM25…˟ GSM25…˟
       <chr>      <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
     1 Asl         1170     361     400     586     626     988     836     535     586     597     938    1035     494     481
     2 Apod       36194   10347    9173   10620   13021   29594   24959   13668   13230   15868   27769   34301   11258   11812
     3 Cyp2d22     4060    1616    1603    1901    2171    3349    3122    2008    2254    2277    2985    3452    1883    2014
     4 Klk6         287     629     641     578     448     195     186    1101     537     567     327     233     742     881
     5 Fcrls         85     233     244     237     180      38      68     375     199     177      89      67     300     233
     6 Slc2a4       782     231     248     265     313     786     528     249     266     357     654     693     271     304
     7 Exd2        1619    2288    2235    2513    2366    1359    1474    3126    2379    2173    1531    1620    2723    2486
     8 Gjc2         288     595     568     551     310     146     186     791     454     370     240     209     745     670
     9 Plp1       43217  101241   96534   58354   53126   27173   28728   98658   61356   61647   38019   38260  102790   82722
    10 Gnb4        1071    1791    1867    1430    1355     798     806    2437    1394    1554     960    1075    2085    1759
    # … with 1,464 more rows, 8 more variables: GSM2545350 <dbl>, GSM2545351 <dbl>, GSM2545352 <dbl>, GSM2545353 <dbl>,
    #   GSM2545354 <dbl>, GSM2545362 <dbl>, GSM2545363 <dbl>, GSM2545380 <dbl>, and abbreviated variable names ¹​GSM2545336,
    #   ²​GSM2545337, ³​GSM2545338, ⁴​GSM2545339, ⁵​GSM2545340, ⁶​GSM2545341, ⁷​GSM2545342, ⁸​GSM2545343, ⁹​GSM2545344, ˟​GSM2545345,
    #   ˟​GSM2545346, ˟​GSM2545347, ˟​GSM2545348, ˟​GSM2545349

{: .output}

To convert the gene expression values from `rna` into a wide-format, we
need to create a new table where the values of the `sample` column would
become the names of column variables.

The key point here is that we are still following a tidy data structure,
but we have **reshaped** the data according to the observations of
interest: expression levels per gene instead of recording them per gene
and per sample.

The opposite transformation would be to transform column names into
values of a new variable.

We can do both these of transformations with two `tidyr` functions,
`pivot_longer()` and `pivot_wider()` (see
[here](https://tidyr.tidyverse.org/dev/articles/pivot.html) for
details).

### Pivoting the data into a wider format

Let’s first select the 3 first columns of `rna` and use `pivot_wider()`
to transform data in a wide-format.

    rna_exp <- rna %>%
      select(gene, sample, expression)
    rna_exp

{: .language-r}

    # A tibble: 32,428 × 3
       gene    sample     expression
       <chr>   <chr>           <dbl>
     1 Asl     GSM2545336       1170
     2 Apod    GSM2545336      36194
     3 Cyp2d22 GSM2545336       4060
     4 Klk6    GSM2545336        287
     5 Fcrls   GSM2545336         85
     6 Slc2a4  GSM2545336        782
     7 Exd2    GSM2545336       1619
     8 Gjc2    GSM2545336        288
     9 Plp1    GSM2545336      43217
    10 Gnb4    GSM2545336       1071
    # … with 32,418 more rows

{: .output}

`pivot_wider` takes three main arguments:

1.  the data to be transformed;
2.  the `names_from` : the column whose values will become new column
    names;
3.  the `values_from`: the column whose values will fill the new
    columns.

![](./figs/pivot_wider.png)

    rna_wide <- rna_exp %>%
      pivot_wider(names_from = sample,
                  values_from = expression)
    rna_wide

{: .language-r}

    # A tibble: 1,474 × 23
       gene    GSM254…¹ GSM25…² GSM25…³ GSM25…⁴ GSM25…⁵ GSM25…⁶ GSM25…⁷ GSM25…⁸ GSM25…⁹ GSM25…˟ GSM25…˟ GSM25…˟ GSM25…˟ GSM25…˟
       <chr>      <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
     1 Asl         1170     361     400     586     626     988     836     535     586     597     938    1035     494     481
     2 Apod       36194   10347    9173   10620   13021   29594   24959   13668   13230   15868   27769   34301   11258   11812
     3 Cyp2d22     4060    1616    1603    1901    2171    3349    3122    2008    2254    2277    2985    3452    1883    2014
     4 Klk6         287     629     641     578     448     195     186    1101     537     567     327     233     742     881
     5 Fcrls         85     233     244     237     180      38      68     375     199     177      89      67     300     233
     6 Slc2a4       782     231     248     265     313     786     528     249     266     357     654     693     271     304
     7 Exd2        1619    2288    2235    2513    2366    1359    1474    3126    2379    2173    1531    1620    2723    2486
     8 Gjc2         288     595     568     551     310     146     186     791     454     370     240     209     745     670
     9 Plp1       43217  101241   96534   58354   53126   27173   28728   98658   61356   61647   38019   38260  102790   82722
    10 Gnb4        1071    1791    1867    1430    1355     798     806    2437    1394    1554     960    1075    2085    1759
    # … with 1,464 more rows, 8 more variables: GSM2545350 <dbl>, GSM2545351 <dbl>, GSM2545352 <dbl>, GSM2545353 <dbl>,
    #   GSM2545354 <dbl>, GSM2545362 <dbl>, GSM2545363 <dbl>, GSM2545380 <dbl>, and abbreviated variable names ¹​GSM2545336,
    #   ²​GSM2545337, ³​GSM2545338, ⁴​GSM2545339, ⁵​GSM2545340, ⁶​GSM2545341, ⁷​GSM2545342, ⁸​GSM2545343, ⁹​GSM2545344, ˟​GSM2545345,
    #   ˟​GSM2545346, ˟​GSM2545347, ˟​GSM2545348, ˟​GSM2545349

{: .output}

Note that by default, the `pivot_wider()` function will add `NA` for
missing values.

Let’s imagine that for some reason, we had some missing expression
values for some genes in certain samples. In the following fictive
example, the gene Cyp2d22 has only one expression value, in GSM2545338
sample.

    rna_with_missing_values

{: .language-r}

    # A tibble: 7 × 3
      gene    sample     expression
      <chr>   <chr>           <dbl>
    1 Asl     GSM2545336       1170
    2 Apod    GSM2545336      36194
    3 Asl     GSM2545337        361
    4 Apod    GSM2545337      10347
    5 Asl     GSM2545338        400
    6 Apod    GSM2545338       9173
    7 Cyp2d22 GSM2545338       1603

{: .output}

By default, the `pivot_wider()` function will add `NA` for missing
values.

    rna_with_missing_values %>%
      pivot_wider(names_from = sample,
                  values_from = expression)

{: .language-r}

    # A tibble: 3 × 4
      gene    GSM2545336 GSM2545337 GSM2545338
      <chr>        <dbl>      <dbl>      <dbl>
    1 Asl           1170        361        400
    2 Apod         36194      10347       9173
    3 Cyp2d22         NA         NA       1603

{: .output}

### Pivoting data into a longer format

In the opposite situation we are using the column names and turning them
into a pair of new variables. One variable represents the column names
as values, and the other variable contains the values previously
associated with the column names.

`pivot_longer()` takes four main arguments:

1.  the data to be transformed;
2.  the `names_to`: the new column name we wish to create and populate
    with the current column names;
3.  the `values_to`: the new column name we wish to create and populate
    with current values;
4.  the names of the columns to be used to populate the `names_to` and
    `values_to` variables (or to drop).

![](./figs/pivot_longer.png)

To recreate `rna_long` from `rna_long` we would create a key called
`sample` and value called `expression` and use all columns except `gene`
for the key variable. Here we drop `gene` column with a minus sign.

Notice how the new variable names are to be quoted here.

    rna_long <- rna_wide %>%
        pivot_longer(names_to = "sample",
                     values_to = "expression",
                     -gene)
    rna_long

{: .language-r}

    # A tibble: 32,428 × 3
       gene  sample     expression
       <chr> <chr>           <dbl>
     1 Asl   GSM2545336       1170
     2 Asl   GSM2545337        361
     3 Asl   GSM2545338        400
     4 Asl   GSM2545339        586
     5 Asl   GSM2545340        626
     6 Asl   GSM2545341        988
     7 Asl   GSM2545342        836
     8 Asl   GSM2545343        535
     9 Asl   GSM2545344        586
    10 Asl   GSM2545345        597
    # … with 32,418 more rows

{: .output}

We could also have used a specification for what columns to include.
This can be useful if you have a large number of identifying columns,
and it’s easier to specify what to gather than what to leave alone. Here
the `starts_with()` function can help to retrieve sample names without
having to list them all! Another possibility would be to use the `:`
operator!

    rna_wide %>%
        pivot_longer(names_to = "sample",
                     values_to = "expression",
                     cols = starts_with("GSM"))

{: .language-r}

    # A tibble: 32,428 × 3
       gene  sample     expression
       <chr> <chr>           <dbl>
     1 Asl   GSM2545336       1170
     2 Asl   GSM2545337        361
     3 Asl   GSM2545338        400
     4 Asl   GSM2545339        586
     5 Asl   GSM2545340        626
     6 Asl   GSM2545341        988
     7 Asl   GSM2545342        836
     8 Asl   GSM2545343        535
     9 Asl   GSM2545344        586
    10 Asl   GSM2545345        597
    # … with 32,418 more rows

{: .output}

    rna_wide %>%
        pivot_longer(names_to = "sample",
                     values_to = "expression",
                     GSM2545336:GSM2545380)

{: .language-r}

    # A tibble: 32,428 × 3
       gene  sample     expression
       <chr> <chr>           <dbl>
     1 Asl   GSM2545336       1170
     2 Asl   GSM2545337        361
     3 Asl   GSM2545338        400
     4 Asl   GSM2545339        586
     5 Asl   GSM2545340        626
     6 Asl   GSM2545341        988
     7 Asl   GSM2545342        836
     8 Asl   GSM2545343        535
     9 Asl   GSM2545344        586
    10 Asl   GSM2545345        597
    # … with 32,418 more rows

{: .output}

Note that if we had missing values in the wide-format, the `NA` would be
included in the new long format.

Remember our previous fictive tibble containing missing values:

    rna_with_missing_values

{: .language-r}

    # A tibble: 7 × 3
      gene    sample     expression
      <chr>   <chr>           <dbl>
    1 Asl     GSM2545336       1170
    2 Apod    GSM2545336      36194
    3 Asl     GSM2545337        361
    4 Apod    GSM2545337      10347
    5 Asl     GSM2545338        400
    6 Apod    GSM2545338       9173
    7 Cyp2d22 GSM2545338       1603

{: .output}

    wide_with_NA <- rna_with_missing_values %>%
      pivot_wider(names_from = sample,
                  values_from = expression)
    wide_with_NA

{: .language-r}

    # A tibble: 3 × 4
      gene    GSM2545336 GSM2545337 GSM2545338
      <chr>        <dbl>      <dbl>      <dbl>
    1 Asl           1170        361        400
    2 Apod         36194      10347       9173
    3 Cyp2d22         NA         NA       1603

{: .output}

    wide_with_NA %>%
        pivot_longer(names_to = "sample",
                     values_to = "expression",
                     -gene)

{: .language-r}

    # A tibble: 9 × 3
      gene    sample     expression
      <chr>   <chr>           <dbl>
    1 Asl     GSM2545336       1170
    2 Asl     GSM2545337        361
    3 Asl     GSM2545338        400
    4 Apod    GSM2545336      36194
    5 Apod    GSM2545337      10347
    6 Apod    GSM2545338       9173
    7 Cyp2d22 GSM2545336         NA
    8 Cyp2d22 GSM2545337         NA
    9 Cyp2d22 GSM2545338       1603

{: .output}

Pivoting to wider and longer formats can be a useful way to balance out
a dataset so every replicate has the same composition.

> ## Question
>
> Subset genes located on X and Y chromosomes from the `rna` data frame
> and spread the data frame with `sex` as columns, `chromosome_name` as
> rows, and the mean expression of genes located in each chromosome as
> the values, as in the following tibble:
>
> ![](./figs/Exercise_pivot_W.png)
>
> You will need to summarize before reshaping!
>
> Let’s first calculate the mean expression level of X and Y linked
> genes from male and female samples…
>
>      rna %>%
>       filter(chromosome_name == "Y" | chromosome_name == "X") %>%
>       group_by(sex, chromosome_name) %>%
>       summarize(mean = mean(expression))
>
> {: .language-r}
>
>     `summarise()` has grouped output by 'sex'. You can override using the `.groups` argument.
>
> {: .output}
>
>     # A tibble: 4 × 3
>     # Groups:   sex [2]
>       sex    chromosome_name  mean
>       <chr>  <chr>           <dbl>
>     1 Female X               3504.
>     2 Female Y                  3 
>     3 Male   X               2497.
>     4 Male   Y               2117.
>
> {: .output}
>
> And pivot the table to wide format
>
>     rna_1 <- rna %>%
>       filter(chromosome_name == "Y" | chromosome_name == "X") %>%
>       group_by(sex, chromosome_name) %>%
>       summarize(mean = mean(expression)) %>%
>       pivot_wider(names_from = sex,
>                   values_from = mean)
>
> {: .language-r}
>
>     `summarise()` has grouped output by 'sex'. You can override using the `.groups` argument.
>
> {: .output}
>
>     rna_1
>
> {: .language-r}
>
>     # A tibble: 2 × 3
>       chromosome_name Female  Male
>       <chr>            <dbl> <dbl>
>     1 X                3504. 2497.
>     2 Y                   3  2117.
>
> {: .output}
>
> Now take that data frame and transform it with `pivot_longer()` so
> each row is a unique `chromosome_name` by `gender` combination.
>
> > ## Solution
> >
> >     rna_1 %>%
> >       pivot_longer(names_to = "gender",
> >                    values_to = "mean",
> >                    - chromosome_name)
> >
> > {: .language-r}
> >
> >     # A tibble: 4 × 3
> >       chromosome_name gender  mean
> >       <chr>           <chr>  <dbl>
> >     1 X               Female 3504.
> >     2 X               Male   2497.
> >     3 Y               Female    3 
> >     4 Y               Male   2117.
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

> ## Question
>
> Use the `rna` dataset to create an expression matrix were each row
> represents the mean expression levels of genes and columns represent
> the different timepoints.
>
> > # Solution
> >
> > Let’s first calculate the mean expression by gene and by time
> >
> >     rna %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_exp = mean(expression))
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     # A tibble: 4,422 × 3
> >     # Groups:   gene [1,474]
> >        gene     time mean_exp
> >        <chr>   <dbl>    <dbl>
> >      1 Aamp        0  4603.  
> >      2 Aamp        4  4870   
> >      3 Aamp        8  4763.  
> >      4 Abca12      0     5.29
> >      5 Abca12      4     4.25
> >      6 Abca12      8     4.14
> >      7 Abcc8       0  2576.  
> >      8 Abcc8       4  2609.  
> >      9 Abcc8       8  2292.  
> >     10 Abhd14a     0   591.  
> >     # … with 4,412 more rows
> >
> > {: .output}
> >
> > before using the pivot\_wider() function
> >
> >     rna_time <- rna %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_exp = mean(expression)) %>%
> >       pivot_wider(names_from = time,
> >                   values_from = mean_exp)
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     rna_time
> >
> > {: .language-r}
> >
> >     # A tibble: 1,474 × 4
> >     # Groups:   gene [1,474]
> >        gene        `0`     `4`     `8`
> >        <chr>     <dbl>   <dbl>   <dbl>
> >      1 Aamp    4603.   4870    4763.  
> >      2 Abca12     5.29    4.25    4.14
> >      3 Abcc8   2576.   2609.   2292.  
> >      4 Abhd14a  591.    547.    432.  
> >      5 Abi2    4881.   4903.   4945.  
> >      6 Abi3bp  1175.   1061.    762.  
> >      7 Abl2    2170.   2078.   2131.  
> >      8 Acadl   2059.   2099    1995.  
> >      9 Acap3   3745    3446.   3431.  
> >     10 Acbd4   1219.   1410.   1668.  
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > Notice that this generates a tibble with some column names starting
> > by a number. If we wanted to select the column corresponding to the
> > timepoints, we could not use the column names directly… What happens
> > when we select the colum 4?
> >
> >     rna %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_exp = mean(expression)) %>%
> >       pivot_wider(names_from = time,
> >                   values_from = mean_exp) %>%
> >       select(gene, 4)
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     # A tibble: 1,474 × 2
> >     # Groups:   gene [1,474]
> >        gene        `8`
> >        <chr>     <dbl>
> >      1 Aamp    4763.  
> >      2 Abca12     4.14
> >      3 Abcc8   2292.  
> >      4 Abhd14a  432.  
> >      5 Abi2    4945.  
> >      6 Abi3bp   762.  
> >      7 Abl2    2131.  
> >      8 Acadl   1995.  
> >      9 Acap3   3431.  
> >     10 Acbd4   1668.  
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > To select the timepoint 4, we would have to quote the column name,
> > with backticks “\`”
> >
> >     rna %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_exp = mean(expression)) %>%
> >       pivot_wider(names_from = time,
> >                   values_from = mean_exp) %>%
> >       select(gene, `4`)
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     # A tibble: 1,474 × 2
> >     # Groups:   gene [1,474]
> >        gene        `4`
> >        <chr>     <dbl>
> >      1 Aamp    4870   
> >      2 Abca12     4.25
> >      3 Abcc8   2609.  
> >      4 Abhd14a  547.  
> >      5 Abi2    4903.  
> >      6 Abi3bp  1061.  
> >      7 Abl2    2078.  
> >      8 Acadl   2099   
> >      9 Acap3   3446.  
> >     10 Acbd4   1410.  
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > Another possibility would be to rename the column, choosing a name
> > that doesn’t start by a number :
> >
> >     rna %>%
> >       group_by(gene, time) %>%
> >       summarize(mean_exp = mean(expression)) %>%
> >       pivot_wider(names_from = time,
> >                   values_from = mean_exp) %>%
> >       rename("time0" = `0`, "time4" = `4`, "time8" = `8`) %>%
> >       select(gene, time4)
> >
> > {: .language-r}
> >
> >     `summarise()` has grouped output by 'gene'. You can override using the `.groups` argument.
> >
> > {: .output}
> >
> >     # A tibble: 1,474 × 2
> >     # Groups:   gene [1,474]
> >        gene      time4
> >        <chr>     <dbl>
> >      1 Aamp    4870   
> >      2 Abca12     4.25
> >      3 Abcc8   2609.  
> >      4 Abhd14a  547.  
> >      5 Abi2    4903.  
> >      6 Abi3bp  1061.  
> >      7 Abl2    2078.  
> >      8 Acadl   2099   
> >      9 Acap3   3446.  
> >     10 Acbd4   1410.  
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

> ## Question
>
> Use the previous data frame containing mean expression levels per
> timepoint and create a new column containing fold-changes between
> timepoint 8 and timepoint 0, and fold-changes between timepoint 8 and
> timepoint 4. Convert this table in a long-format table gathering the
> foldchanges calculated.
>
> > ## Solution
> >
> > starting from the rna\_time tibble:
> >
> >     rna_time
> >
> > {: .language-r}
> >
> >     # A tibble: 1,474 × 4
> >     # Groups:   gene [1,474]
> >        gene        `0`     `4`     `8`
> >        <chr>     <dbl>   <dbl>   <dbl>
> >      1 Aamp    4603.   4870    4763.  
> >      2 Abca12     5.29    4.25    4.14
> >      3 Abcc8   2576.   2609.   2292.  
> >      4 Abhd14a  591.    547.    432.  
> >      5 Abi2    4881.   4903.   4945.  
> >      6 Abi3bp  1175.   1061.    762.  
> >      7 Abl2    2170.   2078.   2131.  
> >      8 Acadl   2059.   2099    1995.  
> >      9 Acap3   3745    3446.   3431.  
> >     10 Acbd4   1219.   1410.   1668.  
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > Calculate FoldChanges:
> >
> >     rna_time %>%
> >       mutate(time_8_vs_0 = `8` / `0`, time_8_vs_4 = `8` / `4`)
> >
> > {: .language-r}
> >
> >     # A tibble: 1,474 × 6
> >     # Groups:   gene [1,474]
> >        gene        `0`     `4`     `8` time_8_vs_0 time_8_vs_4
> >        <chr>     <dbl>   <dbl>   <dbl>       <dbl>       <dbl>
> >      1 Aamp    4603.   4870    4763.         1.03        0.978
> >      2 Abca12     5.29    4.25    4.14       0.784       0.975
> >      3 Abcc8   2576.   2609.   2292.         0.889       0.878
> >      4 Abhd14a  591.    547.    432.         0.731       0.791
> >      5 Abi2    4881.   4903.   4945.         1.01        1.01 
> >      6 Abi3bp  1175.   1061.    762.         0.649       0.719
> >      7 Abl2    2170.   2078.   2131.         0.982       1.03 
> >      8 Acadl   2059.   2099    1995.         0.969       0.950
> >      9 Acap3   3745    3446.   3431.         0.916       0.996
> >     10 Acbd4   1219.   1410.   1668.         1.37        1.18 
> >     # … with 1,464 more rows
> >
> > {: .output}
> >
> > And use the pivot\_longer() function:
> >
> >     rna_time %>%
> >       mutate(time_8_vs_0 = `8` / `0`, time_8_vs_4 = `8` / `4`) %>%
> >       pivot_longer(names_to = "comparisons",
> >                    values_to = "Fold_changes",
> >                    time_8_vs_0:time_8_vs_4)
> >
> > {: .language-r}
> >
> >     # A tibble: 2,948 × 6
> >     # Groups:   gene [1,474]
> >        gene        `0`     `4`     `8` comparisons Fold_changes
> >        <chr>     <dbl>   <dbl>   <dbl> <chr>              <dbl>
> >      1 Aamp    4603.   4870    4763.   time_8_vs_0        1.03 
> >      2 Aamp    4603.   4870    4763.   time_8_vs_4        0.978
> >      3 Abca12     5.29    4.25    4.14 time_8_vs_0        0.784
> >      4 Abca12     5.29    4.25    4.14 time_8_vs_4        0.975
> >      5 Abcc8   2576.   2609.   2292.   time_8_vs_0        0.889
> >      6 Abcc8   2576.   2609.   2292.   time_8_vs_4        0.878
> >      7 Abhd14a  591.    547.    432.   time_8_vs_0        0.731
> >      8 Abhd14a  591.    547.    432.   time_8_vs_4        0.791
> >      9 Abi2    4881.   4903.   4945.   time_8_vs_0        1.01 
> >     10 Abi2    4881.   4903.   4945.   time_8_vs_4        1.01 
> >     # … with 2,938 more rows
> >
> > {: .output}
> >
> > {: .solution} {: .challenge}

## Exporting data

Now that you have learned how to use `dplyr` to extract information from
or summarize your raw data, you may want to export these new data sets
to share them with your collaborators or for archival.

Similar to the `read_csv()` function used for reading CSV files into R,
there is a `write_csv()` function that generates CSV files from data
frames.

Before using `write_csv()`, we are going to create a new folder,
`data_output`, in our working directory that will store this generated
dataset. We don’t want to write generated datasets in the same directory
as our raw data. It’s good practice to keep them separate. The `data`
folder should only contain the raw, unaltered data, and should be left
alone to make sure we don’t delete or modify it. In contrast, our script
will generate the contents of the `data_output` directory, so even if
the files it contains are deleted, we can always re-generate them.

Let’s use `write_csv()` to save the rna\_wide table that we have created
previously.

    write_csv(rna_wide, file = "data_output/rna_wide.csv")

{: .language-r}
