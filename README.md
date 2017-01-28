
`orangetext` is an \#rstats project to keep track of The 🍊 One's speeches and include some code snippets for text analysis on them.

Gladly accepting PRs for legit new transcripts and more analysis scripts.

### Transcripts

-   `2017-01-20-inaugural.txt`
-   `2017-01-21-cia.txt`
-   `2017-01-29-may.txt`

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

    ## Chars:       21458
    ## Letters:     17418
    ## Whitespace:  4040
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       4041
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    118 134 169 188 250 304 479 678 779 942 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 2 25 131 332 959 2584 
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

    ## Observations: 27,896
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 174, 166, 104, 102, 99, 96, 73, 73, 59, 58, 52, 51, 50, ...
    ## $ prop  <dbl> 0.043058649, 0.041078941, 0.025736204, 0.025241277, 0.02...
    ## $ words <chr> "the", "and", "to", "of", "we", "i", "you", "a", "our", ...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 3,785 × 4
    ##        ngram  freq         prop             words
    ##       <fctr> <int>        <dbl>             <chr>
    ## 1  ngrams: 3     8 0.0019806883    youre going to
    ## 2  ngrams: 3     6 0.0014855162       going to be
    ## 3  ngrams: 3     5 0.0012379302     were going to
    ## 4  ngrams: 3     5 0.0012379302        and i said
    ## 5  ngrams: 3     5 0.0012379302        one of the
    ## 6  ngrams: 3     5 0.0012379302      we will make
    ## 7  ngrams: 3     5 0.0012379302 the united states
    ## 8  ngrams: 3     5 0.0012379302 will make america
    ## 9  ngrams: 3     4 0.0009903441       have to get
    ## 10 ngrams: 3     4 0.0009903441      you know the
    ## # ... with 3,775 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 3,953 × 4
    ##        ngram  freq         prop                  words
    ##       <fctr> <int>        <dbl>                  <chr>
    ## 1  ngrams: 4     5 0.0012382368   we will make america
    ## 2  ngrams: 4     4 0.0009905894    will bring back our
    ## 3  ngrams: 4     4 0.0009905894     we will bring back
    ## 4  ngrams: 4     3 0.0007429421  dr martin luther king
    ## 5  ngrams: 4     3 0.0007429421     and youre going to
    ## 6  ngrams: 4     3 0.0007429421     again we will make
    ## 7  ngrams: 4     3 0.0007429421 everybody in this room
    ## 8  ngrams: 4     3 0.0007429421       when i was young
    ## 9  ngrams: 4     3 0.0007429421 on the same wavelength
    ## 10 ngrams: 4     2 0.0004952947        when we were in
    ## # ... with 3,943 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 4,000 × 4
    ##        ngram  freq         prop                      words
    ##       <fctr> <int>        <dbl>                      <chr>
    ## 1  ngrams: 5     4 0.0009908348     we will bring back our
    ## 2  ngrams: 5     3 0.0007431261 again we will make america
    ## 3  ngrams: 5     2 0.0004954174        all the way back to
    ## 4  ngrams: 5     2 0.0004954174         have to get rid of
    ## 5  ngrams: 5     2 0.0004954174        to get you a larger
    ## 6  ngrams: 5     2 0.0004954174 all on the same wavelength
    ## 7  ngrams: 5     2 0.0004954174        the way back to the
    ## 8  ngrams: 5     2 0.0004954174     were going to do great
    ## 9  ngrams: 5     2 0.0004954174       when i was young and
    ## 10 ngrams: 5     2 0.0004954174         we have to get rid
    ## # ... with 3,990 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 4,021 × 4
    ##        ngram  freq         prop                               words
    ##       <fctr> <int>        <dbl>                               <chr>
    ## 1  ngrams: 6     2 0.0004955401 way back to the washington monument
    ## 2  ngrams: 6     2 0.0004955401              we may have to get you
    ## 3  ngrams: 6     2 0.0004955401             all the way back to the
    ## 4  ngrams: 6     2 0.0004955401            have to get you a larger
    ## 5  ngrams: 6     2 0.0004955401               to get rid of isis we
    ## 6  ngrams: 6     2 0.0004955401             have to get rid of isis
    ## 7  ngrams: 6     2 0.0004955401       were going to do great things
    ## 8  ngrams: 6     2 0.0004955401            to get you a larger room
    ## 9  ngrams: 6     2 0.0004955401     were all on the same wavelength
    ## 10 ngrams: 6     2 0.0004955401         a million and a half people
    ## # ... with 4,011 more rows
