---
title: 'discogger (day one)'
description: 'Building an R interface to the Discogs API.'
date: '2018-06-07'
slug: discogger-day-one
tags: ['music', 'r']
keywords: ['discogs', 'discogger', 'r', 'api']
image: https://raw.githubusercontent.com/rbind/ewenme/master/static/blog/2018-06-07-discogger-day-one_files/figure-html/unnamed-chunk-4-1.png
draft: no
---

I've been working on a new R package.
[**`discogger`**](https://github.com/ewenme/discogger) is an open-source effort,
at an experimental stage of development, providing straightforward tools to help
out with Discogs' [API v2.0](https://www.discogs.com/developers/#).
[Discogs](https://www.discogs.com) is a crowd-sourced music database and
marketplace, a proper treasure trove for record diggers and music lovers to
learn more about what they're jamming to. A better route into this data for R
users should yield some nice results.

## discogger 101

First up, you'll need to register a Discogs application via
[https://www.discogs.com/settings/developers](https://www.discogs.com/settings/developers).
Doing so will grant you a "personal access token" (accessible at the same link
above) which you can store as an environment variable (_DISCOGS_API_TOKEN_)
using the `discogs_api_token()` function. All functions in the `discogger`
package will automatically look for your token and prompt entry if it isn't
found.

The `discogger` interface to Discogs data currently spans
[Database](https://www.discogs.com/developers/#page:database) (releases,
artists, labels) and
[User Collection](https://www.discogs.com/developers/#page:user-collection) API
requests, with a view to growing functionality in these spaces and eventually
expanding into
[Marketplace](https://www.discogs.com/developers/#page:marketplace) (inventory,
orders). The next section will demo how these functions can be put in to use,
and chained together for quietly impressive analyses.

## Diggin' in to Dance Mania

[Dance Mania](http://dancemaniarecords.com/) is about as iconic as it gets when
it comes to Chicago House record labels (you hopefully got a flavour by clicking
on the name...). `discogger` is well equipped to retrieve record label
information, with `discogs_label()` first up.

```r
# load packages
library(discogger)
library(tidyverse)

# get DM label info
discogs_label(label_id = 314)
```

```
## <Discogs labels/314>
## Preview: 1 of 12 results.
## List of 1
##  $ id: int 314
```

Hopefully you learnt something new from that output, but it's not particularly
ready for any analysis. I'm quite interested in getting more detailed metadata
around Dance Mania's 12" releases, which can be done with
`discogs_label_releases()`.

```r
# get DM releases
dm_releases <- discogs_label_releases(label_id = 314)

# extract request content
dm_releases_df <- dm_releases$content %>%
  # return 12" releases only
  dplyr::filter(grepl('12"', format)) %>%
  as_tibble()

dm_releases_df
```

```
## # A tibble: 599 × 14
##    status   format     catno  thumb    resource_url  title     id  year artist
##    <chr>    <chr>      <chr>  <chr>    <chr>         <chr>  <int> <int> <chr>
##  1 Accepted "12\""     14040  https:/… https://api.… Hous… 6.40e4  1987 The Hou…
##  2 Accepted "12\""     51002… https:/… https://api.… What… 1.09e6  1985 The Bro…
##  3 Accepted "12\""     B.C. … https:/… https://api.… Hous… 4.21e5  1987 The Hou…
##  4 Accepted "12\""     B.C. … https:/… https://api.… Hous… 1.97e5  1987 The Hou…
##  5 Accepted "12\""     BASIC… https:/… https://api.… Akce… 3.64e4  1998 DJ Deeon
##  6 Accepted "12\""     D.J. … https:/… https://api.… Frea… 6.75e4  1996 D.J. Fu…
##  7 Accepted "12\", TP" D.M. … https:/… https://api.… This… 5.17e6  1988 Mello D.
##  8 Accepted "12\""     D.M. … https:/… https://api.… This… 4.27e5  1988 Mello D.
##  9 Accepted "12\""     DM 003 https:/… https://api.… Hous… 7.30e3  1986 The Hou…
## 10 Accepted "12\""     DM 004 https:/… https://api.… Hard… 1.4 e3  1987 Duane &…
## # … with 589 more rows, and 5 more variables:
## #   stats.community.in_wantlist <int>, stats.community.in_collection <int>,
## #   stats.user.in_wantlist <int>, stats.user.in_collection <int>,
## #   label_id <dbl>
```

The tidy tibble format returned is much better suited to analysis in R, but the
content I'm after is not quite there. Turns out community metrics, such as the
number of users who own or have marked a record as one they "want", are
available from a records release page (not it's label release listing).
`discogs_release()` can be used, in conjunction with `map()` (from the
[`purrr`](https://purrr.tidyverse.org/) package), to iterate through releases
and collect this information.

```r
# get DM release info
dm_releases_info <- map(dm_releases_df$id, discogs_release)

# extract release content
dm_releases_content <- map(dm_releases_info, "content")

# extract fields relevant for community metrics analysis
dm_community_metrics <- tibble(
  title = map_chr(dm_releases_content, "title"),
  artist = map_chr(dm_releases_content, "artists_sort"),
  year = map_int(dm_releases_content, "year"),
  have = map_int(dm_releases_content, c("community", "have")),
  want = map_int(dm_releases_content, c("community", "want"))
  ) %>%
  # for each record...
  group_by(title, artist) %>%
  filter(
    # keep the one most users have...
    have == max(have),
    # ...and rm releases with missing release year
    year >= 1985
    ) %>%
  ungroup()

dm_community_metrics
```

```
## # A tibble: 313 × 5
##    title                      artist                            year  have  want
##    <chr>                      <chr>                            <int> <int> <int>
##  1 What's That                Browns, The                       1985    86   275
##  2 Akceier 8                  DJ Deeon                          1998   214    82
##  3 Freaky Style Take: 2       DJ Funk                           1996   221   255
##  4 This X-Mas Rap             Mello D.                          1988     3    36
##  5 This Christmas (Rap)       Mello D.                          1988    41    98
##  6 House Nation               Housemaster Boyz, The And Rude …  1986  1379  1490
##  7 Hard Core (On The One)     Duane & Co.                       1987   536   342
##  8 Jack My Body               Yellow House                      1987   401   358
##  9 Frequency (Out Of Control) Lil' Louis                        1987     4   457
## 10 Insane                     Suburban Boyz                     1988   340   604
## # … with 303 more rows
```

With this output, a visualisation can be knocked up to explore which Dance Mania
12-inches Discogs users want or own.

```r
# load viz helpers
library(scico)
library(ewenthemes)
library(ggrepel)

# plot
ggplot(dm_community_metrics, aes(x=have, y=want)) +
  geom_point(aes(colour=year)) +
  geom_text_repel(aes(label=paste0(artist, " - ", title)), size=3,
                  data = subset(dm_community_metrics, have >= 800 | want >= 1000),
                  family = "IBM Plex Sans") +
  labs(title="Dance Mania 12\"s on Discogs",
       subtitle="How many Discogs user's own/want Dance Mania (1985 - 2018) 12-inches",
       x = "Own it", y = "Want it",
       caption="source: Discogs | made by @ewen_") +
  theme_ewen_rs(grid = FALSE, subtitle_size = 11, subtitle_margin = 20) +
  scico::scale_colour_scico(palette = "lajolla") +
  guides(col = guide_colourbar(direction = "horizontal", title = "Release year",
                               barheight = 0.5, barwidth = 10, title.vjust = 1)) +
  theme(legend.position = "bottom")
```

![](index_files/figure-html/unnamed-chunk-4-1.svg)

## Close

For more on installation, development status and for (very welcome)
contributions or issues, check in at the
[Github repo](https://github.com/ewenme/discogger). I'll look to list features I
intend to introduce there - feel free to beat me to the punch on any of those.

> Music is a language, you see, a universal language. Sun Ra
