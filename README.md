<!-- README.md is generated from README.Rmd. Please edit that file -->

skimr <img src="inst/figures/skimmer_hex.png" align="right" height="139" />
===========================================================================

[![Build
Status](https://travis-ci.org/ropensci/skimr.svg?branch=master)](https://travis-ci.org/ropensci/skimr)
[![codecov](https://codecov.io/gh/ropensci/skimr/branch/master/graph/badge.svg)](https://codecov.io/gh/ropenscilabs/skimr)
[![](https://badges.ropensci.org/175_status.svg)](https://github.com/ropensci/onboarding/issues/175)

`skimr` provides a frictionless approach to summary statistics which
conforms to the [principle of least
surprise](https://en.wikipedia.org/wiki/Principle_of_least_astonishment),
displaying summary statistics the user can skim quickly to understand
their data. It handles different data types and returns a `skim_df`
object which can be included in a pipeline or displayed nicely for the
human reader.

Installation
------------

The current released version of `skimr` can be installed from CRAN. If
you wish to install the current build of the next release you can do so
using the following:

    # install.packages("devtools")
    devtools::install_github("ropensci/skimr")

The APIs for this branch should be considered reasonably stable but
still subject to change if an issue is discovered.

To install the version with the most recent changes that have not yet
been incorporated in the master branch (and may not be):

    devtools::install_github("ropensci/skimr", ref = "develop")

Do not rely on APIs from the develop branch, as they are likely to
change.

Skim statistics in the console
------------------------------

`skimr`:

-   Provides a larger set of statistics than `summary()`, including
    missing, complete, n, and sd.
-   reports each data types separately
-   handles dates, logicals, and a variety of other types
-   supports spark-bar and spark-line based on [Hadley Wickham’s pillar
    package](https://github.com/hadley/pillar).

### Separates variables by class:

    skim(chickwts)

    ## ── Data Summary ────────────────────────
    ##                            Value
    ## Name                    chickwts
    ## Number of rows                71
    ## Number of columns              2
    ##                                 
    ## Column type frequency:          
    ##   factor                       1
    ##   numeric                      1
    ##                                 
    ## Group variables             None
    ## 
    ## ── Variable type: factor ──────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n ordered n_unique top_counts                        
    ## 1 feed                0       71    71 FALSE          6 soy: 14, cas: 12, lin: 12, sun: 12
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ## 1 weight              0       71    71  261.  78.1   108  204.   258  324.   423 ▆▆▇▇▃

### Presentation is in a compact horizontal format:

    skim(iris)

    ## ── Data Summary ────────────────────────
    ##                         Value
    ## Name                     iris
    ## Number of rows            150
    ## Number of columns           5
    ##                              
    ## Column type frequency:       
    ##   factor                    1
    ##   numeric                   4
    ##                              
    ## Group variables          None
    ## 
    ## ── Variable type: factor ──────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n ordered n_unique top_counts               
    ## 1 Species             0      150   150 FALSE          3 set: 50, ver: 50, vir: 50
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ## 1 Sepal.Length        0      150   150  5.84 0.828   4.3   5.1  5.8    6.4   7.9 ▆▇▇▅▂
    ## 2 Sepal.Width         0      150   150  3.06 0.436   2     2.8  3      3.3   4.4 ▁▆▇▂▁
    ## 3 Petal.Length        0      150   150  3.76 1.77    1     1.6  4.35   5.1   6.9 ▇▁▆▇▂
    ## 4 Petal.Width         0      150   150  1.20 0.762   0.1   0.3  1.3    1.8   2.5 ▇▁▇▅▃

### Built in support for strings, lists and other column classes

    skim(dplyr::starwars)

    ## ── Data Summary ────────────────────────
    ##                                   Value
    ## Name                    dplyr::starwars
    ## Number of rows                       87
    ## Number of columns                    13
    ##                                        
    ## Column type frequency:                 
    ##   character                           7
    ##   list                                3
    ##   numeric                             3
    ##                                        
    ## Group variables                    None
    ## 
    ## ── Variable type: character ───────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n   min   max empty n_unique whitespace
    ## 1 name                0       87    87     3    21     0       87          0
    ## 2 hair_color          5       82    87     4    13     0       12          0
    ## 3 skin_color          0       87    87     3    19     0       31          0
    ## 4 eye_color           0       87    87     3    13     0       15          0
    ## 5 gender              3       84    87     4    13     0        4          0
    ## 6 homeworld          10       77    87     4    14     0       48          0
    ## 7 species             5       82    87     3    14     0       37          0
    ## 
    ## ── Variable type: list ────────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n n_unique min_length max_length
    ## 1 films               0       87    87       24          1          7
    ## 2 vehicles            0       87    87       11          0          2
    ## 3 starships           0       87    87       17          0          5
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ## 1 height              6       81    87 174.   34.8    66 167     180 191     264 ▁▁▇▅▁
    ## 2 mass               28       59    87  97.3 169.     15  55.6    79  84.5  1358 ▇▁▁▁▁
    ## 3 birth_year         44       43    87  87.6 155.      8  35      52  72     896 ▇▁▁▁▁

### Has a useful summary function

    skim(iris) %>%
      summary()

    ## ── Data Summary ────────────────────────
    ##                         Value
    ## Name                     iris
    ## Number of rows            150
    ## Number of columns           5
    ##                              
    ## Column type frequency:       
    ##   factor                    1
    ##   numeric                   4
    ##                              
    ## Group variables          None

### Individual columns can be selected using tidyverse-style selectors

    skim(iris, Sepal.Length, Petal.Length)

    ## ── Data Summary ────────────────────────
    ##                         Value
    ## Name                     iris
    ## Number of rows            150
    ## Number of columns           5
    ##                              
    ## Column type frequency:       
    ##   numeric                   2
    ##                              
    ## Group variables          None
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ## 1 Sepal.Length        0      150   150  5.84 0.828   4.3   5.1  5.8    6.4   7.9 ▆▇▇▅▂
    ## 2 Petal.Length        0      150   150  3.76 1.77    1     1.6  4.35   5.1   6.9 ▇▁▆▇▂

### Handles grouped data

`skim()` can handle data that has been grouped using
`dplyr::group_by()`.

    iris %>%
      dplyr::group_by(Species) %>%
      skim()

    ## ── Data Summary ────────────────────────
    ##                              Value
    ## Name                    Piped data
    ## Number of rows                 150
    ## Number of columns                5
    ##                                   
    ## Column type frequency:            
    ##   numeric                        4
    ##                                   
    ## Group variables            Species
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##    skim_variable Species    missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ##  * <chr>         <fct>        <int>    <int> <int> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <chr>
    ##  1 Sepal.Length  setosa           0       50    50 5.01  0.352   4.3  4.8   5     5.2    5.8 ▃▃▇▅▁
    ##  2 Sepal.Length  versicolor       0       50    50 5.94  0.516   4.9  5.6   5.9   6.3    7   ▂▇▆▃▃
    ##  3 Sepal.Length  virginica        0       50    50 6.59  0.636   4.9  6.22  6.5   6.9    7.9 ▁▃▇▃▂
    ##  4 Sepal.Width   setosa           0       50    50 3.43  0.379   2.3  3.2   3.4   3.68   4.4 ▁▃▇▅▂
    ##  5 Sepal.Width   versicolor       0       50    50 2.77  0.314   2    2.52  2.8   3      3.4 ▁▅▆▇▂
    ##  6 Sepal.Width   virginica        0       50    50 2.97  0.322   2.2  2.8   3     3.18   3.8 ▂▆▇▅▁
    ##  7 Petal.Length  setosa           0       50    50 1.46  0.174   1    1.4   1.5   1.58   1.9 ▁▃▇▃▁
    ##  8 Petal.Length  versicolor       0       50    50 4.26  0.470   3    4     4.35  4.6    5.1 ▂▂▇▇▆
    ##  9 Petal.Length  virginica        0       50    50 5.55  0.552   4.5  5.1   5.55  5.88   6.9 ▃▇▇▃▂
    ## 10 Petal.Width   setosa           0       50    50 0.246 0.105   0.1  0.2   0.2   0.3    0.6 ▇▂▂▁▁
    ## 11 Petal.Width   versicolor       0       50    50 1.33  0.198   1    1.2   1.3   1.5    1.8 ▅▇▃▆▁
    ## 12 Petal.Width   virginica        0       50    50 2.03  0.275   1.4  1.8   2     2.3    2.5 ▂▇▆▅▇

### Behaves nicely in pipelines

    iris %>%
      skim() %>%
      dplyr::filter(numeric.sd > 1)

    ## ── Data Summary ────────────────────────
    ##                              Value
    ## Name                    Piped data
    ## Number of rows                 150
    ## Number of columns                5
    ##                                   
    ## Column type frequency:            
    ##   numeric                        1
    ##                                   
    ## Group variables               None
    ## 
    ## ── Variable type: numeric ─────────────────────────────────────────────────────────────────────
    ##   skim_variable missing complete     n  mean    sd    p0   p25   p50   p75  p100 hist 
    ## 1 Petal.Length        0      150   150  3.76  1.77     1   1.6  4.35   5.1   6.9 ▇▁▆▇▂

Knitted results
---------------

Simply skimming a data frame will produce the horizontal print layout
shown above. We provide a `knit_print` method for the types of objects
in this package so that similar results are produced in documents. To
use this, make sure the `skimmed` object is the last item in your code
chunk.

    faithful %>%
      skim()

<table style="width:51%;">
<caption>Data summary</caption>
<colgroup>
<col style="width: 34%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;">Name Number of rows Number of columns</td>
<td style="text-align: left;">Piped data 272 2</td>
</tr>
<tr class="even">
<td style="text-align: left;">Column type frequency: numeric</td>
<td style="text-align: left;">2</td>
</tr>
<tr class="odd">
<td style="text-align: left;">Group variables</td>
<td style="text-align: left;">None</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">missing</th>
<th style="text-align: right;">complete</th>
<th style="text-align: right;">n</th>
<th style="text-align: right;">mean</th>
<th style="text-align: right;">sd</th>
<th style="text-align: right;">p0</th>
<th style="text-align: right;">p25</th>
<th style="text-align: right;">p50</th>
<th style="text-align: right;">p75</th>
<th style="text-align: right;">p100</th>
<th style="text-align: left;">hist</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">eruptions</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">272</td>
<td style="text-align: right;">272</td>
<td style="text-align: right;">3.49</td>
<td style="text-align: right;">1.14</td>
<td style="text-align: right;">1.6</td>
<td style="text-align: right;">2.16</td>
<td style="text-align: right;">4</td>
<td style="text-align: right;">4.45</td>
<td style="text-align: right;">5.1</td>
<td style="text-align: left;">▇▂▂▇▇</td>
</tr>
<tr class="even">
<td style="text-align: left;">waiting</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">272</td>
<td style="text-align: right;">272</td>
<td style="text-align: right;">70.90</td>
<td style="text-align: right;">13.59</td>
<td style="text-align: right;">43.0</td>
<td style="text-align: right;">58.00</td>
<td style="text-align: right;">76</td>
<td style="text-align: right;">82.00</td>
<td style="text-align: right;">96.0</td>
<td style="text-align: left;">▃▃▂▇▂</td>
</tr>
</tbody>
</table>

Customizing skimr
-----------------

Although skimr provides opinionated defaults, it is highly customizable.
Users can specify their own statistics, change the formatting of
results, create statistics for new classes and develop skimmers for data
structures that are not data frames.

### Specify your own statistics and classes

Users can specify their own statistics using a list combined with the
`skim_with()` function factory. `skim_with()` returns a new `skim`
function that can be called on your data. You can use this factory to
produce summaries for any type of column within your data.

Assignment within a call to `skim_with()` relies on a helper function,
`sfl` or `skimr` function list. This is a light wrapper around
`dplyr::funs()`. It will automatically generate names from the provided
values.

By default, functions in the `sfl` call are appended to the default
skimmers.

    my_skim <- skim_with(numeric = sfl(mad))
    my_skim(iris, Sepal.Length)

<table style="width:43%;">
<caption>Data summary</caption>
<colgroup>
<col style="width: 34%" />
<col style="width: 8%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;">Name Number of rows Number of columns</td>
<td style="text-align: left;">iris 150 5</td>
</tr>
<tr class="even">
<td style="text-align: left;">Column type frequency: numeric</td>
<td style="text-align: left;">1</td>
</tr>
<tr class="odd">
<td style="text-align: left;">Group variables</td>
<td style="text-align: left;">None</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">missing</th>
<th style="text-align: right;">complete</th>
<th style="text-align: right;">n</th>
<th style="text-align: right;">mean</th>
<th style="text-align: right;">sd</th>
<th style="text-align: right;">p0</th>
<th style="text-align: right;">p25</th>
<th style="text-align: right;">p50</th>
<th style="text-align: right;">p75</th>
<th style="text-align: right;">p100</th>
<th style="text-align: left;">hist</th>
<th style="text-align: right;">mad</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">Sepal.Length</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">150</td>
<td style="text-align: right;">150</td>
<td style="text-align: right;">5.84</td>
<td style="text-align: right;">0.83</td>
<td style="text-align: right;">4.3</td>
<td style="text-align: right;">5.1</td>
<td style="text-align: right;">5.8</td>
<td style="text-align: right;">6.4</td>
<td style="text-align: right;">7.9</td>
<td style="text-align: left;">▆▇▇▅▂</td>
<td style="text-align: right;">1.04</td>
</tr>
</tbody>
</table>

But you can also use the dummy argument pattern from `dplyr::funs` to
set particular function arguments. Setting the `append = FALSE` argument
uses only those functions that you’ve provided.

    my_skim <- skim_with(
      numeric = sfl(iqr = IQR, p99 = ~ quantile(., probs = .99)), append = FALSE)
    my_skim(iris, Sepal.Length)

<table style="width:43%;">
<caption>Data summary</caption>
<colgroup>
<col style="width: 34%" />
<col style="width: 8%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;">Name Number of rows Number of columns</td>
<td style="text-align: left;">iris 150 5</td>
</tr>
<tr class="even">
<td style="text-align: left;">Column type frequency: numeric</td>
<td style="text-align: left;">1</td>
</tr>
<tr class="odd">
<td style="text-align: left;">Group variables</td>
<td style="text-align: left;">None</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">iqr</th>
<th style="text-align: right;">p99</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">Sepal.Length</td>
<td style="text-align: right;">1.3</td>
<td style="text-align: right;">7.7</td>
</tr>
</tbody>
</table>

And you can default skimmers by setting them to `NULL`.

    my_skim <- skim_with(numeric = sfl(hist = NULL))
    my_skim(iris, Sepal.Length)

<table style="width:43%;">
<caption>Data summary</caption>
<colgroup>
<col style="width: 34%" />
<col style="width: 8%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: left;">Name Number of rows Number of columns</td>
<td style="text-align: left;">iris 150 5</td>
</tr>
<tr class="even">
<td style="text-align: left;">Column type frequency: numeric</td>
<td style="text-align: left;">1</td>
</tr>
<tr class="odd">
<td style="text-align: left;">Group variables</td>
<td style="text-align: left;">None</td>
</tr>
</tbody>
</table>

**Variable type: numeric**

<table>
<thead>
<tr class="header">
<th style="text-align: left;">skim_variable</th>
<th style="text-align: right;">missing</th>
<th style="text-align: right;">complete</th>
<th style="text-align: right;">n</th>
<th style="text-align: right;">mean</th>
<th style="text-align: right;">sd</th>
<th style="text-align: right;">p0</th>
<th style="text-align: right;">p25</th>
<th style="text-align: right;">p50</th>
<th style="text-align: right;">p75</th>
<th style="text-align: right;">p100</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">Sepal.Length</td>
<td style="text-align: right;">0</td>
<td style="text-align: right;">150</td>
<td style="text-align: right;">150</td>
<td style="text-align: right;">5.84</td>
<td style="text-align: right;">0.83</td>
<td style="text-align: right;">4.3</td>
<td style="text-align: right;">5.1</td>
<td style="text-align: right;">5.8</td>
<td style="text-align: right;">6.4</td>
<td style="text-align: right;">7.9</td>
</tr>
</tbody>
</table>

### Skimming other objects

`skimr` has summary functions for the following types of data by
default:

-   `numeric` (which includes both `double` and `integer`)
-   `character`
-   `factor`
-   `logical`
-   `complex`
-   `Date`
-   `POSIXct`
-   `ts`
-   `AsIs`

`skimr` also provides a small API for writing packages that provide
their own default summary functions for data types not covered above. It
relies on R S3 methods for the `get_skimmers` function. This function
should return a `sfl`, similar to customization within `skim_with()`,
but you should also provide a value for the `class` argument. Here’s an
example.

    get_skimmers.my_data_type <- function(column) {
      sfl(
        .class = "my_data_type",
        missing = n_missing,
        complete = n_complete,
        p99 = quantile(., probs = .99))
    }

Limitations of current version
------------------------------

We are aware that there are issues with rendering the inline histograms
and line charts in various contexts, some of which are described below.

### Support for spark histograms

There are known issues with printing the spark-histogram characters when
printing a data frame. For example, `"▂▅▇"` is printed as
`"<U+2582><U+2585><U+2587>"`. This longstanding problem [originates in
the low-level
code](http://r.789695.n4.nabble.com/Unicode-display-problem-with-data-frames-under-Windows-td4707639.html)
for printing dataframes. While some cases have been addressed, there
are, for example, reports of this issue in Emacs ESS.

This means that while `skimr` can render the histograms to the console
and in RMarkdown documents, it cannot in other circumstances. This
includes:

-   rendering a `skimr` data frame within `pander()`
-   converting a `skimr` data frame to a vanilla R data frame, but
    tibbles render correctly

One workaround for showing these characters in Windows is to set the
CTYPE part of your locale to Chinese/Japanese/Korean with
`Sys.setlocale("LC_CTYPE", "Chinese"). The helper function`fix\_windows\_histograms()\`
does this for you.

### Printing spark histograms and line graphs in knitted documents

Spark-bar and spark-line work in the console, but may not work when you
knit them to a specific document format. The same session that produces
a correctly rendered HTML document may produce an incorrectly rendered
PDF, for example. This issue can generally be addressed by changing
fonts to one with good building block (for histograms) and Braille
support (for line graphs). For example, the open font “DejaVu Sans” from
the `extrafont` package supports these. You may also want to try
wrapping your results in `knitr::kable()`. Please see the vignette on
using fonts for details.

Displays in documents of different types will vary. For example, one
user found that the font “Yu Gothic UI Semilight” produced consistent
results for Microsoft Word and Libre Office Write.

Contributing
------------

We welcome issue reports and pull requests, including potentially adding
support for commonly used variable classes. However, in general, we
encourage users to take advantage of skimr’s flexibility to add their
own customized classes. Please see the [contributing](CONTRIBUTING.md)
and [conduct](CONDUCT.md) documents.

[![ropenci\_footer](https://ropensci.org/public_images/ropensci_footer.png)](https://ropensci.org)
