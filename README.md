
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

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
  stri_trim_both() %>%
  discard(equals, "") %>%
  paste0(collapse=" ") %>%
  stri_replace_all_regex("[[:space:]]+", " ") %>%
  preprocess(case="lower", remove.punct=TRUE,
             remove.numbers=TRUE, fix.spacing=TRUE) -> texts
```

What have we got:

``` r
string.summary(texts)
```

    ## Chars:       20133
    ## Letters:     16322
    ## Whitespace:  3811
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       3812
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    106 128 162 173 224 287 451 648 749 884 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 1 25 119 310 896 2453 
    ##              4 1 1 1 1 1 1

The 1-grams are kinda useless but this makes a big tibble for 1:8-grams.

``` r
map_df(1:8, ~ngram(texts, n=.x) %>%
         get.phrasetable() %>%
         tbl_df() %>%
         rename(words=ngrams) %>%
         mutate(words=stri_trim_both(words)) %>%
         mutate(ngram=sprintf("ngrams: %s", .x))) %>%
  mutate(ngram=factor(ngram, levels=unique(ngram))) -> grams
```

``` r
glimpse(grams)
```

    ## Observations: 26,321
    ## Variables: 4
    ## $ words <chr> "the", "and", "to", "of", "we", "i", "you", "a", "that",...
    ## $ freq  <int> 163, 155, 99, 96, 95, 94, 69, 68, 55, 51, 50, 49, 48, 47...
    ## $ prop  <dbl> 0.042759706, 0.040661070, 0.025970619, 0.025183631, 0.02...
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
