Clean Qualtrics Export
======================

Load packages
-------------

``` r
library(dplyr)
library(purrr)
library(tidyr)
library(ggplot2)
library(hash)
```

Constants
---------

``` r
QUALTRICS_FILENAME = "qualtrics.tsv"
```

Set datapath and load `shlab.imgct`
-----------------------------------

Begin by setting the working directory and important top-level paths to
data and loading necessary packages.

-   NOTE: This will be changed to dynamically account for the package
    `shlab.imgct` via its GitHub instance later. For now, it is using
    development loading.

``` r
# Set the working directory to be part of S Drive (may make dynamic later?)
# Whilst not dynamic, change for own session if mount point is not equivalent on
# local machine
shared_dir <- "~/Projects/shlab/mounts/imgct"
package_dir <- "~/Projects/shlab"

datapath <- file.path(shared_dir, "csn_images")
imgct_package_path <- file.path(package_dir, "shlab.imgct")

# Make sure that devtools, tidyverse are installed before this call
devtools::load_all(imgct_package_path)
```

Load Qualtrics TSV
------------------

Using the convience method `shlab.imgct::load_qualtrics_tsv` will load a
TSV export of Qualtrics response data collected from the image
categorization task. (Please note that the output of this raw dataset is
hidden to maintain participant privacy.)

``` r
qualtrics_export <- shlab.imgct::load_qualtrics_tsv(datapath)
```

Parse Qualtrics Export
----------------------

Remove unnecessary columns of data from the Qualtrics exported data, and
remove participant rows in which the task was not complete. The output
of this function will show the first five participants.

``` r
qualtrics_export_parsed <- shlab.imgct::parse_qualtrics_export(qualtrics_export)

knitr::kable(head(qualtrics_export_parsed, 5))
```

| participant\_id | imageBlock | X1\_Q10 | X2\_Q10 | X3\_Q10 | X4\_Q10 | X5\_Q10 | X6\_Q10 | X7\_Q10 | X8\_Q10 | X9\_Q10 | X10\_Q10 | X11\_Q10 | X12\_Q10 | X13\_Q10 | X14\_Q10 | X15\_Q10 | X16\_Q10 | X17\_Q10 | X18\_Q10 | X19\_Q10 | X20\_Q10 | X21\_Q10 | X22\_Q10 | X23\_Q10 | X24\_Q10 | X25\_Q10 | X26\_Q10 | X27\_Q10 | X28\_Q10 | X29\_Q10 | X30\_Q10 | X31\_Q10 | X32\_Q10 | X33\_Q10 | X34\_Q10 | X35\_Q10 | X36\_Q10 | X37\_Q10 | X38\_Q10 | X39\_Q10 | X40\_Q10 | X41\_Q10 | X42\_Q10 | X43\_Q10 | X44\_Q10 | X45\_Q10 | X46\_Q10 | X47\_Q10 | X48\_Q10 | X49\_Q10 | X50\_Q10 | X51\_Q10 | X52\_Q10 | X53\_Q10 | X54\_Q10 | X55\_Q10 | X56\_Q10 | X57\_Q10 | X58\_Q10 | X59\_Q10 | X60\_Q10 | X61\_Q10 | X62\_Q10 | X63\_Q10 | X64\_Q10 | X65\_Q10 | X66\_Q10 | X67\_Q10 | X68\_Q10 | X69\_Q10 | X70\_Q10 | X71\_Q10 | X72\_Q10 | X73\_Q10 | X74\_Q10 | X75\_Q10 | X76\_Q10 | X77\_Q10 | X78\_Q10 | X79\_Q10 | X80\_Q10 | X81\_Q10 | X82\_Q10 | X83\_Q10 | X84\_Q10 | X85\_Q10 | X86\_Q10 | X87\_Q10 | X88\_Q10 | X89\_Q10 | X90\_Q10 | X91\_Q10 | X92\_Q10 | X93\_Q10 | X94\_Q10 | X95\_Q10 | X96\_Q10 | X97\_Q10 | X98\_Q10 | X99\_Q10 | X100\_Q10 | X101\_Q10 | X102\_Q10 | X103\_Q10 | X104\_Q10 | X105\_Q10 | X106\_Q10 | X107\_Q10 | X108\_Q10 | X109\_Q10 | X110\_Q10 | X111\_Q10 | X112\_Q10 | X113\_Q10 | X114\_Q10 | X115\_Q10 | X116\_Q10 | X117\_Q10 | X118\_Q10 | X119\_Q10 | X120\_Q10 | X121\_Q10 | X122\_Q10 | X123\_Q10 | X124\_Q10 | X125\_Q10 | X126\_Q10 | X127\_Q10 | X128\_Q10 | X129\_Q10 | X130\_Q10 | X131\_Q10 | X132\_Q10 | X133\_Q10 | X134\_Q10 | X135\_Q10 | X136\_Q10 | X137\_Q10 | X138\_Q10 | X139\_Q10 | X140\_Q10 | X141\_Q10 | X142\_Q10 | X143\_Q10 | X144\_Q10 | X145\_Q10 | X146\_Q10 | X147\_Q10 | X148\_Q10 | X149\_Q10 | X150\_Q10 | X151\_Q10 | X152\_Q10 | X153\_Q10 | X154\_Q10 | X155\_Q10 | X156\_Q10 | X157\_Q10 | X158\_Q10 | X159\_Q10 | X160\_Q10 | X161\_Q10 | X162\_Q10 | X163\_Q10 | X164\_Q10 | X165\_Q10 | X166\_Q10 | X167\_Q10 | X168\_Q10 | X169\_Q10 | X170\_Q10 | X171\_Q10 | X172\_Q10 | X173\_Q10 | X174\_Q10 | X175\_Q10 | X176\_Q10 | X177\_Q10 | X178\_Q10 | X179\_Q10 | X180\_Q10 | X181\_Q10 | X182\_Q10 | X183\_Q10 | X184\_Q10 | X185\_Q10 | X186\_Q10 | X187\_Q10 | X188\_Q10 | X189\_Q10 | X190\_Q10 | X191\_Q10 | X192\_Q10 | X193\_Q10 | X194\_Q10 | X195\_Q10 | X196\_Q10 | X197\_Q10 | X198\_Q10 | X199\_Q10 | X200\_Q10 | X201\_Q10 | X202\_Q10 | X203\_Q10 | X204\_Q10 | X205\_Q10 |
|:----------------|:-----------|:--------|:--------|:--------|:--------|:--------|:--------|:--------|:--------|:--------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|:----------|
| ICT\_001        | 01         | 1       | 5       | 4       | 4       | 1       | 3       | 1       | 1       | 1       | 1        | 3        | 5        | 1        | 1        | 2        | 1        | 1        | 4        | 1        | 2        | 1        | 1        | 2        | 1        | 1        | 1        | 2        | 1        | 2        | 2        | 1        | 3        | 3        | 3        | 2        | 3        | 2        | 1        | 3        | 1        | 1        | 1        | 2        | 3        | 4        | 2        | 3        | 5        | 3        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 4        | 4        | 4        | 2        | 4        | 3        | 4        | 1        | 5        | 5        | 2        | 2        | 1        | 3        | 3        | 3        | 1        | 4        | 1        | 1        | 2        | 3        | 1        | 3        | 4        | 3        | 1        | 1        | 3        | 2        | 4        | 1        | 4        | 4        | 4        | 1        | 3        | 2        | 5        | 1        | 1        | 3        | 1        | 4         | 1         | 1         | 1         | 1         | 3         | 4         | 3         | 1         | 2         | 2         | 1         | 4         | 3         | 1         | 1         | 5         | 3         | 2         | 1         | 3         | 1         | 4         | 3         | 3         | 1         | 1         | 1         | 4         | 1         | 4         | 3         | 4         | 1         | 1         | 4         | 1         | 1         | 4         | 4         | 3         | 1         | 3         | 2         | 2         | 3         | 1         | 3         | 2         | 3         | 1         | 1         | 4         | 1         | 3         | 1         | 1         | 1         | 2         | 3         | 1         | 1         | 4         | 2         | 3         | 1         | 1         | 1         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 4         | 2         | 3         | 2         | 4         | 4         | 2         | 5         | 1         | 1         | 5         | 4         | 1         | 4         | 3         | 1         | 1         | 1         | 1         | 3         | 1         | 5         | 3         | 3         | 2         | 1         | 4         | 4         |
| ICT\_002        | 01         | 1       | 3       | 3       | 3       | 3       | 3       | 1       | 1       | 1       | 1        | 4        | 3        | 1        | 1        | 1        | 1        | 1        | 4        | 4        | 4        | 2        | 1        | 5        | 1        | 1        | 1        | 2        | 2        | 2        | 2        | 3        | 3        | 3        | 1        | 2        | 3        | 2        | 1        | 3        | 1        | 1        | 1        | 1        | 1        | 3        | 2        | 4        | 4        | 4        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 2        | 1        | 2        | 4        | 4        | 4        | 1        | 4        | 2        | 2        | 1        | 1        | 4        | 4        | 3        | 1        | 4        | 1        | 1        | 2        | 3        | 1        | 3        | 1        | 3        | 1        | 1        | 1        | 2        | 4        | 1        | 1        | 4        | 4        | 1        | 4        | 3        | 4        | 1        | 1        | 4        | 1        | 4         | 1         | 1         | 1         | 1         | 1         | 4         | 3         | 2         | 2         | 2         | 1         | 1         | 3         | 1         | 1         | 3         | 3         | 2         | 1         | 3         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 3         | 1         | 1         | 4         | 4         | 1         | 1         | 4         | 1         | 1         | 1         | 1         | 1         | 1         | 1         | 2         | 2         | 1         | 1         | 3         | 2         | 1         | 1         | 2         | 1         | 3         | 2         | 1         | 1         | 1         | 2         | 3         | 1         | 3         | 1         | 1         | 4         | 3         | 1         | 1         | 1         | 1         | 2         | 3         | 1         | 4         | 2         | 3         | 4         | 4         | 4         | 4         | 3         | 2         | 4         | 4         | 2         | 1         | 1         | 1         | 1         | 1         | 2         | 2         | 4         | 1         | 3         | 4         | 4         | 3         | 1         | 5         | 2         | 1         | 3         | 1         | 2         | 3         |
| ICT\_003        | 01         | 1       | 5       | 4       | 3       | 1       | 3       | 1       | 1       | 1       | 1        | 3        | 3        | 1        | 1        | 2        | 1        | 1        | 4        | 1        | 2        | 1        | 1        | 2        | 1        | 1        | 1        | 2        | 1        | 2        | 2        | 1        | 3        | 3        | 1        | 2        | 3        | 2        | 1        | 3        | 1        | 1        | 1        | 1        | 3        | 4        | 2        | 4        | 4        | 3        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 2        | 1        | 2        | 4        | 4        | 4        | 1        | 4        | 4        | 2        | 2        | 1        | 4        | 3        | 3        | 1        | 4        | 1        | 1        | 2        | 3        | 1        | 3        | 1        | 3        | 1        | 1        | 1        | 2        | 4        | 1        | 1        | 3        | 4        |          | 4        | 2        | 4        | 1        | 1        | 4        | 1        | 4         | 1         | 1         | 1         | 1         | 3         | 4         | 3         | 2         | 2         | 2         | 1         | 1         | 3         | 1         | 1         | 1         | 3         | 2         | 1         | 3         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 2         | 1         | 4         | 4         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 1         | 1         | 1         | 1         | 2         | 2         | 3         | 1         | 3         | 2         | 1         | 1         | 1         | 1         | 1         | 3         | 1         | 1         | 1         | 3         | 3         | 1         | 1         | 4         | 2         | 3         | 1         | 1         | 1         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 4         | 3         | 3         | 2         | 4         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 3         | 1         | 1         | 1         | 1         | 3         | 1         | 5         | 3         | 3         | 2         | 1         | 4         | 4         |
| ICT\_004        | 01         | 1       | 3       | 3       | 3       | 1       | 3       | 1       | 1       | 1       | 1        | 3        | 4        | 1        | 1        | 2        | 1        | 1        | 4        | 1        | 2        | 1        | 1        | 3        | 1        | 1        | 1        | 2        | 1        | 2        | 2        | 1        | 3        | 4        | 3        | 2        | 3        | 2        | 1        | 3        | 1        | 1        | 1        | 1        | 3        | 3        | 2        | 3        | 4        | 3        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 2        | 1        | 2        | 4        | 3        | 4        | 1        | 3        | 2        | 2        | 2        | 1        | 4        | 3        | 3        | 1        | 4        | 1        | 1        | 2        | 3        | 1        | 3        | 1        | 3        | 1        | 1        | 3        | 2        | 4        | 1        | 1        | 3        | 4        | 1        | 4        | 2        | 4        | 1        | 1        | 4        | 1        | 4         | 1         | 1         | 1         | 1         | 3         | 4         | 3         | 2         | 2         | 2         | 1         | 4         | 3         | 1         | 1         | 1         | 3         | 2         | 1         | 3         | 1         | 1         | 3         | 4         | 1         | 1         | 1         | 3         | 1         | 3         | 4         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 1         | 1         | 1         | 1         | 2         | 2         | 3         | 1         | 3         | 2         | 3         | 1         | 1         | 1         | 1         | 3         | 1         | 1         | 1         | 2         | 3         | 1         | 1         | 4         | 2         | 3         | 1         | 1         | 1         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 4         | 2         | 3         | 2         | 4         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 3         | 1         | 1         | 1         | 3         | 3         | 1         | 5         | 3         | 5         | 2         | 1         | 4         | 4         |
| ICT\_005        | 01         | 1       | 3       | 4       | 3       | 1       | 3       | 1       | 1       | 1       | 1        | 3        | 4        | 1        | 1        | 2        | 1        | 1        | 3        | 1        | 2        | 1        | 1        | 3        | 1        | 1        | 1        | 2        | 1        | 2        | 2        | 1        | 4        | 3        | 1        | 2        | 3        | 2        | 1        | 3        | 1        | 1        | 1        | 1        | 3        | 3        | 2        | 3        | 4        | 3        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 1        | 4        | 1        | 2        | 4        | 4        | 4        | 1        | 4        | 4        | 2        | 2        | 1        | 3        | 3        | 3        | 1        | 4        | 1        | 1        | 2        | 3        | 1        | 3        | 1        | 3        | 1        | 1        | 3        | 2        | 4        | 1        | 1        | 4        | 4        | 1        | 3        | 3        | 4        | 1        | 1        | 4        | 1        | 4         | 1         | 1         | 1         | 1         | 1         | 4         | 3         | 2         | 2         | 2         | 1         | 1         | 3         | 4         | 1         | 4         | 3         | 2         | 1         | 3         | 1         | 4         | 3         | 4         | 1         | 1         | 1         | 4         | 1         | 4         | 4         | 4         | 1         | 1         | 4         | 3         | 1         | 4         | 1         | 1         | 1         | 1         | 2         | 2         | 1         | 1         | 3         | 2         | 1         | 1         | 1         | 4         | 1         | 3         | 1         | 4         | 1         | 2         | 3         | 1         | 1         | 4         | 2         | 3         | 1         | 1         | 1         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 1         | 1         | 4         | 2         | 3         | 2         | 4         | 4         | 2         | 1         | 1         | 1         | 1         | 4         | 1         | 4         | 3         | 1         | 1         | 1         | 1         | 3         | 1         | 5         | 4         | 3         | 2         | 1         | 4         | 4         |

Clean Qualtrics Export
----------------------

The two above demonstrations are included within the function to clean
Qualtrics exported survey data for this task. Additionally, the `clean`
function is a convenience on top of `clean_qualtrics_export` that
(currently) only allows for Qualtrics TSV response data. Within the
call, TXT files of each image block are loaded to rename columns for
each block of image categorization rating responses, as Qualtrics also
unfortunately replaces columns with each block of images surveyed.

``` r
# Here, we demonstrate the underlying Qualtrics-specific method
shlab.imgct::clean_qualtrics_export(datapath, filename = QUALTRICS_FILENAME)
```

Using the convenient abstraction `clean`, we can load, parse, and clean
each block of image categorization rating responses across participants.
This will, notably, remove any participant that that has errors in their
responses, too. If successful, the cleaned blocks will be saved to the
`~/datapath/clean` directory.

``` r
shlab.imgct::clean(datapath, filename = QUALTRICS_FILENAME)
```

    ## [1] "Success! Your clean blocks were saved to  ~/Projects/shlab/mounts/imgct/csn_images/clean"
