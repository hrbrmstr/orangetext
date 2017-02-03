
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

### Transcripts

-   `2016-01-19-presidential-candidacy-anouncement-NewYorkCity-NY.txt`
-   `2016-08-31-immigration-Phoenix-AZ.txt`
-   `2016-10-13-addressing-sexual-assault-WestPalmBeach-FL.txt`
-   `2017-01-20-inaugural.txt`
-   `2017-01-21-cia.txt`
-   `2017-01-28-may.txt`
-   `2017-01-29-weekly-address.txt`
-   `2017-01-31-gorsuch.txt`
-   `2017-02-01-black-history-month.txt`
-   `2017-02-01-blackhistorymonth.txt`

### Sample code

``` r
library(ngram)
library(tidyverse)
library(magrittr)
library(ggalt)
library(hrbrmisc)
library(stringi)
library(rprojroot)
```

Read all the speeches in:

``` r
rprojroot::find_rstudio_root_file() %>%
  file.path("data", "speeches") %>%
  list.files("*.txt", full.names=TRUE) %>%
  map(read_lines) %>%
  flatten_chr() %>%
  stri_enc_toascii() %>%  
  stri_trim_both() %>%
  discard(equals, "") %>%
  paste0(collapse=" ") %>%
  stri_replace_all_regex("[[:space:]]+", " ") %>%
  preprocess(case="lower", remove.punct=TRUE,
             remove.numbers=TRUE, fix.spacing=TRUE) -> texts
```

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

    ## Warning in stri_enc_toascii(.): invalid UTF-8 codepoint definition. fixing

What have we got:

``` r
string.summary(texts)
```

    ## Chars:       128349
    ## Letters:     104053
    ## Whitespace:  23607
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       23608
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    722 870 912 1000 1761 1882 2909 3827 4693 5032 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 9 20 186 824 2152 5902 14458 
    ##              3 1 1 1 1 1 1 1

The 1-grams are kinda useless but this makes a big tibble for 1:8-grams.

``` r
map_df(1:8, ~ngram(texts, n=.x) %>%
         get.phrasetable() %>%
         tbl_df() %>%
         rename(words=ngrams) %>%
         mutate(words=stri_trim_both(words)) %>%
         mutate(ngram=sprintf("ngrams: %s", .x))) %>%
  mutate(ngram=factor(ngram, levels=unique(ngram))) %>% 
  select(ngram, freq, prop, words) -> grams
```

``` r
glimpse(grams)
```

    ## Observations: 150,366
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 990, 908, 646, 487, 465, 427, 391, 358, 302, 301, 294, 2...
    ## $ prop  <dbl> 0.041934937, 0.038461538, 0.027363606, 0.020628600, 0.01...
    ## $ words <chr> "the", "and", "to", "of", "a", "i", "we", "that", "you",...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 20,284 × 4
    ##        ngram  freq         prop               words
    ##       <fctr> <int>        <dbl>               <chr>
    ## 1  ngrams: 3    28 0.0011861391   the united states
    ## 2  ngrams: 3    25 0.0010590528         going to be
    ## 3  ngrams: 3    25 0.0010590528          one of the
    ## 4  ngrams: 3    21 0.0008896043       were going to
    ## 5  ngrams: 3    20 0.0008472422          we have to
    ## 6  ngrams: 3    18 0.0007625180          by the way
    ## 7  ngrams: 3    16 0.0006777938        not going to
    ## 8  ngrams: 3    15 0.0006354317          and by the
    ## 9  ngrams: 3    15 0.0006354317 the american people
    ## 10 ngrams: 3    15 0.0006354317            a lot of
    ## # ... with 20,274 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 22,076 × 4
    ##        ngram  freq         prop                    words
    ##       <fctr> <int>        <dbl>                    <chr>
    ## 1  ngrams: 4    12 0.0005083669           and by the way
    ## 2  ngrams: 4     9 0.0003812752       the new york times
    ## 3  ngrams: 4     9 0.0003812752          we are going to
    ## 4  ngrams: 4     9 0.0003812752     of the united states
    ## 5  ngrams: 4     9 0.0003812752       all over the place
    ## 6  ngrams: 4     9 0.0003812752      thank you thank you
    ## 7  ngrams: 4     8 0.0003389112     we will make america
    ## 8  ngrams: 4     8 0.0003389112      we have people that
    ## 9  ngrams: 4     7 0.0002965473 make america great again
    ## 10 ngrams: 4     7 0.0002965473          i want to thank
    ## # ... with 22,066 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 22,607 × 4
    ##        ngram  freq         prop                         words
    ##       <fctr> <int>        <dbl>                         <chr>
    ## 1  ngrams: 5     5 0.0002118285            all you have to do
    ## 2  ngrams: 5     5 0.0002118285        the new york times and
    ## 3  ngrams: 5     4 0.0001694628 will make america great again
    ## 4  ngrams: 5     4 0.0001694628          we will vote for the
    ## 5  ngrams: 5     4 0.0001694628        we will bring back our
    ## 6  ngrams: 5     4 0.0001694628   movement the likes of which
    ## 7  ngrams: 5     4 0.0001694628    we will make america great
    ## 8  ngrams: 5     4 0.0001694628       we have people that are
    ## 9  ngrams: 5     4 0.0001694628             you have to do is
    ## 10 ngrams: 5     3 0.0001270971           and i will say this
    ## # ... with 22,597 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 22,769 × 4
    ##        ngram  freq         prop                              words
    ##       <fctr> <int>        <dbl>                              <chr>
    ## 1  ngrams: 6     4 1.694700e-04              all you have to do is
    ## 2  ngrams: 6     4 1.694700e-04   we will make america great again
    ## 3  ngrams: 6     3 1.271025e-04 make america great again thank you
    ## 4  ngrams: 6     3 1.271025e-04             you have to do is look
    ## 5  ngrams: 6     3 1.271025e-04       were going to bring our jobs
    ## 6  ngrams: 6     3 1.271025e-04       going to bring our jobs back
    ## 7  ngrams: 6     3 1.271025e-04        to bring our jobs back home
    ## 8  ngrams: 6     3 1.271025e-04              have to do is look at
    ## 9  ngrams: 6     3 1.271025e-04           said to me the other day
    ## 10 ngrams: 6     2 8.473499e-05 it\032s one of the favorite things
    ## # ... with 22,759 more rows
