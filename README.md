
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

### Transcripts

-   `2017-01-20-inaugural.txt`
-   `2017-01-21-cia.txt`
-   `2017-01-28-may.txt`
-   `2017-01-29-weekly-address.txt`

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

    ## Chars:       23194
    ## Letters:     18853
    ## Whitespace:  4341
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       4342
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    137 151 176 202 272 323 507 733 836 1005 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 3 37 149 359 1032 2754 
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

    ## Observations: 29,949
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 189, 176, 115, 109, 107, 98, 78, 76, 64, 63, 54, 53, 53,...
    ## $ prop  <dbl> 0.043528328, 0.040534316, 0.026485491, 0.025103639, 0.02...
    ## $ words <chr> "the", "and", "to", "of", "we", "i", "a", "you", "that",...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 4,066 × 4
    ##        ngram  freq        prop             words
    ##       <fctr> <int>       <dbl>             <chr>
    ## 1  ngrams: 3     8 0.001843318    youre going to
    ## 2  ngrams: 3     6 0.001382488       going to be
    ## 3  ngrams: 3     5 0.001152074     were going to
    ## 4  ngrams: 3     5 0.001152074        and i said
    ## 5  ngrams: 3     5 0.001152074        one of the
    ## 6  ngrams: 3     5 0.001152074      we will make
    ## 7  ngrams: 3     5 0.001152074        we have to
    ## 8  ngrams: 3     5 0.001152074 the united states
    ## 9  ngrams: 3     5 0.001152074 will make america
    ## 10 ngrams: 3     4 0.000921659       have to get
    ## # ... with 4,056 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 4,249 × 4
    ##        ngram  freq         prop                  words
    ##       <fctr> <int>        <dbl>                  <chr>
    ## 1  ngrams: 4     5 0.0011523392   we will make america
    ## 2  ngrams: 4     4 0.0009218714    will bring back our
    ## 3  ngrams: 4     4 0.0009218714     we will bring back
    ## 4  ngrams: 4     3 0.0006914035  dr martin luther king
    ## 5  ngrams: 4     3 0.0006914035     and youre going to
    ## 6  ngrams: 4     3 0.0006914035     again we will make
    ## 7  ngrams: 4     3 0.0006914035 everybody in this room
    ## 8  ngrams: 4     3 0.0006914035       when i was young
    ## 9  ngrams: 4     3 0.0006914035 on the same wavelength
    ## 10 ngrams: 4     2 0.0004609357        when we were in
    ## # ... with 4,239 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 4,298 × 4
    ##        ngram  freq         prop                      words
    ##       <fctr> <int>        <dbl>                      <chr>
    ## 1  ngrams: 5     4 0.0009220839     we will bring back our
    ## 2  ngrams: 5     3 0.0006915629 again we will make america
    ## 3  ngrams: 5     2 0.0004610420        all the way back to
    ## 4  ngrams: 5     2 0.0004610420         have to get rid of
    ## 5  ngrams: 5     2 0.0004610420  you and god bless america
    ## 6  ngrams: 5     2 0.0004610420        to get you a larger
    ## 7  ngrams: 5     2 0.0004610420 all on the same wavelength
    ## 8  ngrams: 5     2 0.0004610420        the way back to the
    ## 9  ngrams: 5     2 0.0004610420     were going to do great
    ## 10 ngrams: 5     2 0.0004610420       when i was young and
    ## # ... with 4,288 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 4,320 × 4
    ##        ngram  freq         prop                               words
    ##       <fctr> <int>        <dbl>                               <chr>
    ## 1  ngrams: 6     2 0.0004611483 way back to the washington monument
    ## 2  ngrams: 6     2 0.0004611483              we may have to get you
    ## 3  ngrams: 6     2 0.0004611483             all the way back to the
    ## 4  ngrams: 6     2 0.0004611483            have to get you a larger
    ## 5  ngrams: 6     2 0.0004611483               to get rid of isis we
    ## 6  ngrams: 6     2 0.0004611483     bless you and god bless america
    ## 7  ngrams: 6     2 0.0004611483             have to get rid of isis
    ## 8  ngrams: 6     2 0.0004611483       were going to do great things
    ## 9  ngrams: 6     2 0.0004611483            to get you a larger room
    ## 10 ngrams: 6     2 0.0004611483         god bless you and god bless
    ## # ... with 4,310 more rows
