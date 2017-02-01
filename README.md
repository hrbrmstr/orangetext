
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

### Transcripts

-   `2017-01-20-inaugural.txt`
-   `2017-01-21-cia.txt`
-   `2017-01-28-may.txt`
-   `2017-01-29-weekly-address.txt`
-   `2017-01-31-gorsuch.txt`

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

    ## Chars:       27394
    ## Letters:     22294
    ## Whitespace:  5096
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       5097
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    170 184 215 235 340 392 579 859 958 1165 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 5 45 180 449 1224 3185 
    ##              4 1 1 1 1 1 1

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

    ## Observations: 35,127
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 226, 205, 136, 132, 119, 108, 96, 83, 72, 71, 62, 62, 55...
    ## $ prop  <dbl> 0.044339808, 0.040219737, 0.026682362, 0.025897587, 0.02...
    ## $ words <chr> "the", "and", "to", "of", "i", "we", "a", "you", "our", ...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 4,772 × 4
    ##        ngram  freq         prop               words
    ##       <fctr> <int>        <dbl>               <chr>
    ## 1  ngrams: 3     8 0.0015701668   the united states
    ## 2  ngrams: 3     8 0.0015701668      youre going to
    ## 3  ngrams: 3     6 0.0011776251         going to be
    ## 4  ngrams: 3     6 0.0011776251 the american people
    ## 5  ngrams: 3     6 0.0011776251          one of the
    ## 6  ngrams: 3     5 0.0009813543       were going to
    ## 7  ngrams: 3     5 0.0009813543          and i said
    ## 8  ngrams: 3     5 0.0009813543      of our country
    ## 9  ngrams: 3     5 0.0009813543        we will make
    ## 10 ngrams: 3     5 0.0009813543          we have to
    ## # ... with 4,762 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 4,990 × 4
    ##        ngram  freq         prop                   words
    ##       <fctr> <int>        <dbl>                   <chr>
    ## 1  ngrams: 4     5 0.0009815469    we will make america
    ## 2  ngrams: 4     4 0.0007852375     will bring back our
    ## 3  ngrams: 4     4 0.0007852375      we will bring back
    ## 4  ngrams: 4     3 0.0005889282     thank you very much
    ## 5  ngrams: 4     3 0.0005889282   dr martin luther king
    ## 6  ngrams: 4     3 0.0005889282      and youre going to
    ## 7  ngrams: 4     3 0.0005889282      again we will make
    ## 8  ngrams: 4     3 0.0005889282 bless america thank you
    ## 9  ngrams: 4     3 0.0005889282  everybody in this room
    ## 10 ngrams: 4     3 0.0005889282    of the united states
    ## # ... with 4,980 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 5,048 × 4
    ##        ngram  freq         prop                       words
    ##       <fctr> <int>        <dbl>                       <chr>
    ## 1  ngrams: 5     4 0.0007853917      we will bring back our
    ## 2  ngrams: 5     3 0.0005890438 god bless america thank you
    ## 3  ngrams: 5     3 0.0005890438  again we will make america
    ## 4  ngrams: 5     2 0.0003926959         all the way back to
    ## 5  ngrams: 5     2 0.0003926959          have to get rid of
    ## 6  ngrams: 5     2 0.0003926959   you and god bless america
    ## 7  ngrams: 5     2 0.0003926959         to get you a larger
    ## 8  ngrams: 5     2 0.0003926959  all on the same wavelength
    ## 9  ngrams: 5     2 0.0003926959         the way back to the
    ## 10 ngrams: 5     2 0.0003926959      were going to do great
    ## # ... with 5,038 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 5,072 × 4
    ##        ngram  freq        prop                               words
    ##       <fctr> <int>       <dbl>                               <chr>
    ## 1  ngrams: 6     2 0.000392773 way back to the washington monument
    ## 2  ngrams: 6     2 0.000392773              we may have to get you
    ## 3  ngrams: 6     2 0.000392773             all the way back to the
    ## 4  ngrams: 6     2 0.000392773            have to get you a larger
    ## 5  ngrams: 6     2 0.000392773     and god bless america thank you
    ## 6  ngrams: 6     2 0.000392773     you and god bless america thank
    ## 7  ngrams: 6     2 0.000392773               to get rid of isis we
    ## 8  ngrams: 6     2 0.000392773     bless you and god bless america
    ## 9  ngrams: 6     2 0.000392773             have to get rid of isis
    ## 10 ngrams: 6     2 0.000392773       were going to do great things
    ## # ... with 5,062 more rows
