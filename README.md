
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
-   `2017-02-032-national-prayer.txt`
-   `2017-02-03-weekly-address.txt`
-   `2017-02-07-major_cities_chiefs_association_conference`#
## Sample code

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

What have we got:

``` r
string.summary(texts)
```

    ## Chars:       127786
    ## Letters:     103672
    ## Whitespace:  23463
    ## Punctuation: 0
    ## Digits:      0
    ## Words:       23464
    ## Sentences:   0
    ## Lines:       1 
    ## Wordlens:    728 869 898 1004 1784 1861 2879 3794 4634 5013 
    ##              1 1 1 1 1 1 1 1 1 1 
    ## Senlens:     0 
    ##              10 
    ## Syllens:     0 8 19 192 829 2174 5859 14331 
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

    ## Observations: 154,149
    ## Variables: 4
    ## $ ngram <fctr> ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ngrams: 1, ...
    ## $ freq  <int> 984, 903, 654, 492, 458, 420, 383, 355, 311, 299, 291, 2...
    ## $ prop  <dbl> 0.041936584, 0.038484487, 0.027872486, 0.020968292, 0.01...
    ## $ words <chr> "the", "and", "to", "of", "a", "i", "we", "that", "our",...

``` r
filter(grams, ngram=="ngrams: 3")
```

    ## # A tibble: 20,791 × 4
    ##        ngram  freq         prop               words
    ##       <fctr> <int>        <dbl>               <chr>
    ## 1  ngrams: 3    30 0.0012786634   the united states
    ## 2  ngrams: 3    27 0.0011507970         going to be
    ## 3  ngrams: 3    24 0.0010229307          one of the
    ## 4  ngrams: 3    21 0.0008950644       were going to
    ## 5  ngrams: 3    20 0.0008524422          we have to
    ## 6  ngrams: 3    18 0.0007671980          by the way
    ## 7  ngrams: 3    16 0.0006819538        not going to
    ## 8  ngrams: 3    15 0.0006393317          and by the
    ## 9  ngrams: 3    15 0.0006393317 the american people
    ## 10 ngrams: 3    15 0.0006393317      of our country
    ## # ... with 20,781 more rows

``` r
filter(grams, ngram=="ngrams: 4")
```

    ## # A tibble: 22,630 × 4
    ##        ngram  freq         prop                    words
    ##       <fctr> <int>        <dbl>                    <chr>
    ## 1  ngrams: 4    12 0.0005114871           and by the way
    ## 2  ngrams: 4    10 0.0004262393     of the united states
    ## 3  ngrams: 4     9 0.0003836154       the new york times
    ## 4  ngrams: 4     9 0.0003836154          we are going to
    ## 5  ngrams: 4     9 0.0003836154       all over the place
    ## 6  ngrams: 4     9 0.0003836154      thank you thank you
    ## 7  ngrams: 4     8 0.0003409914     we will make america
    ## 8  ngrams: 4     8 0.0003409914      we have people that
    ## 9  ngrams: 4     7 0.0002983675 make america great again
    ## 10 ngrams: 4     6 0.0002557436           is going to be
    ## # ... with 22,620 more rows

``` r
filter(grams, ngram=="ngrams: 5")
```

    ## # A tibble: 23,181 × 4
    ##        ngram  freq         prop                           words
    ##       <fctr> <int>        <dbl>                           <chr>
    ## 1  ngrams: 5     5 0.0002131287              all you have to do
    ## 2  ngrams: 5     5 0.0002131287          the new york times and
    ## 3  ngrams: 5     4 0.0001705030   will make america great again
    ## 4  ngrams: 5     4 0.0001705030            we will vote for the
    ## 5  ngrams: 5     4 0.0001705030             that i can tell you
    ## 6  ngrams: 5     4 0.0001705030          we will bring back our
    ## 7  ngrams: 5     4 0.0001705030 the united states supreme court
    ## 8  ngrams: 5     4 0.0001705030     movement the likes of which
    ## 9  ngrams: 5     4 0.0001705030      we will make america great
    ## 10 ngrams: 5     4 0.0001705030         we have people that are
    ## # ... with 23,171 more rows

``` r
filter(grams, ngram=="ngrams: 6")
```

    ## # A tibble: 23,350 × 4
    ##        ngram  freq         prop                              words
    ##       <fctr> <int>        <dbl>                              <chr>
    ## 1  ngrams: 6     4 0.0001705103              all you have to do is
    ## 2  ngrams: 6     4 0.0001705103   we will make america great again
    ## 3  ngrams: 6     3 0.0001278827 make america great again thank you
    ## 4  ngrams: 6     3 0.0001278827             you have to do is look
    ## 5  ngrams: 6     3 0.0001278827       were going to bring our jobs
    ## 6  ngrams: 6     3 0.0001278827    bless you and god bless america
    ## 7  ngrams: 6     3 0.0001278827       going to bring our jobs back
    ## 8  ngrams: 6     3 0.0001278827        god bless you and god bless
    ## 9  ngrams: 6     3 0.0001278827        to bring our jobs back home
    ## 10 ngrams: 6     3 0.0001278827              have to do is look at
    ## # ... with 23,340 more rows
