
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

### Transcripts

-   `2017-01-20-inaugural.txt`
-   `2017-01-21-cia.txt`
-   `2017-01-28-may.txt`
-   `2017-01-29-weekly-address.txt`
-   `2017-01-31-gorsuch.txt`
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

    ## Chars:       31208
    ## Letters:     25375
    ## Whitespace:  5829
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       5830
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    196 201 252 260 371 447 680 990 1120 1313 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 2 6 48 206 491 1414 3649 
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

    ## Observations: 40,023
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 256, 233, 150, 145, 138, 122, 114, 92, 81, 76, 75, 68, 6...
    ## $ prop  <dbl> 0.043910806, 0.039965695, 0.025728988, 0.024871355, 0.02...
    ## $ words <chr> "the", "and", "to", "of", "i", "we", "a", "you", "that",...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 5,449 × 4
    ##        ngram  freq         prop               words
    ##       <fctr> <int>        <dbl>               <chr>
    ## 1  ngrams: 3     8 0.0013726836          one of the
    ## 2  ngrams: 3     8 0.0013726836   the united states
    ## 3  ngrams: 3     8 0.0013726836      youre going to
    ## 4  ngrams: 3     7 0.0012010981  martin luther king
    ## 5  ngrams: 3     7 0.0012010981           i want to
    ## 6  ngrams: 3     6 0.0010295127       were going to
    ## 7  ngrams: 3     6 0.0010295127         going to be
    ## 8  ngrams: 3     6 0.0010295127 the american people
    ## 9  ngrams: 3     6 0.0010295127         going to do
    ## 10 ngrams: 3     5 0.0008579272          and i said
    ## # ... with 5,439 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 5,709 × 4
    ##        ngram  freq         prop                   words
    ##       <fctr> <int>        <dbl>                   <chr>
    ## 1  ngrams: 4     5 0.0008580745   dr martin luther king
    ## 2  ngrams: 4     5 0.0008580745    we will make america
    ## 3  ngrams: 4     4 0.0006864596     will bring back our
    ## 4  ngrams: 4     4 0.0006864596      we will bring back
    ## 5  ngrams: 4     4 0.0006864596         i want to thank
    ## 6  ngrams: 4     3 0.0005148447     thank you very much
    ## 7  ngrams: 4     3 0.0005148447      and youre going to
    ## 8  ngrams: 4     3 0.0005148447      again we will make
    ## 9  ngrams: 4     3 0.0005148447 bless america thank you
    ## 10 ngrams: 4     3 0.0005148447        were going to do
    ## # ... with 5,699 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 5,779 × 4
    ##        ngram  freq         prop                       words
    ##       <fctr> <int>        <dbl>                       <chr>
    ## 1  ngrams: 5     4 0.0006865774      we will bring back our
    ## 2  ngrams: 5     3 0.0005149331 god bless america thank you
    ## 3  ngrams: 5     3 0.0005149331  again we will make america
    ## 4  ngrams: 5     2 0.0003432887         all the way back to
    ## 5  ngrams: 5     2 0.0003432887          have to get rid of
    ## 6  ngrams: 5     2 0.0003432887   you and god bless america
    ## 7  ngrams: 5     2 0.0003432887         to get you a larger
    ## 8  ngrams: 5     2 0.0003432887  all on the same wavelength
    ## 9  ngrams: 5     2 0.0003432887         the way back to the
    ## 10 ngrams: 5     2 0.0003432887   dr martin luther king but
    ## # ... with 5,769 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 5,805 × 4
    ##        ngram  freq         prop                               words
    ##       <fctr> <int>        <dbl>                               <chr>
    ## 1  ngrams: 6     2 0.0003433476 way back to the washington monument
    ## 2  ngrams: 6     2 0.0003433476              we may have to get you
    ## 3  ngrams: 6     2 0.0003433476             all the way back to the
    ## 4  ngrams: 6     2 0.0003433476            have to get you a larger
    ## 5  ngrams: 6     2 0.0003433476     and god bless america thank you
    ## 6  ngrams: 6     2 0.0003433476     you and god bless america thank
    ## 7  ngrams: 6     2 0.0003433476               to get rid of isis we
    ## 8  ngrams: 6     2 0.0003433476     bless you and god bless america
    ## 9  ngrams: 6     2 0.0003433476             have to get rid of isis
    ## 10 ngrams: 6     2 0.0003433476       were going to do great things
    ## # ... with 5,795 more rows
