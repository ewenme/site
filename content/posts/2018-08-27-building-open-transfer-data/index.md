---
title: 'building open football player transfer data'
description:
  'Collating player transfers to and from football clubs in major European
  leagues.'
date: '2018-08-27'
slug: building-open-football-player-transfer-data
tags: ['football', 'open-data', 'r']
keywords: ['football', 'transfers', 'open data', 'r', 'web scraping']
draft: no
---

Around this time last year I ran
[a post on European football transfers](https://ewen.io/2017/09/07/unravelling-the-european-football-transfer-window/),
taking in the 2017/18 season's summer window. I got a bit of an itch to refresh
this work when the 2018/19 window hit. The aforementioned itch led to me getting
in too deep and scraping all major European league transfers going back to the
year 2000, naturally.

Here, I tell a short story about how this open data was built and showcases some
visualisation pieces that utilised that effort.[^code] Hopefully this can
encourage others to share whatever neat stuff they tap into.

## Building the data

I eventually settled on ~~scraping~~ using
[the Guardian's Transfer Interactive](https://www.theguardian.com/football/ng-interactive/2017/jun/22/transfer-window-2017-every-deal-in-europes-top-five-leagues)
to power my previous work. This source included transfer timestamps, which
allowed for some intra-window time series stuff - it remains handily
[hosted by Tom Worville in a public, flat-file format](https://github.com/Worville/guardian_transfers).
However, it isn't really set up for investigating historical trends as the
Guardian has only run this interactive since 2017, as far as I can tell.

Enter [Transfermarkt](https://www.transfermarkt.co.uk/), a data goldmine of
player transfers for a number of major European leagues (e.g. English Premier
League, Spanish La Liga, Italian Serie A) and some other oddities (my personal
favourite is
[this list of father/son combos for national teams - glorious and appreciated](https://www.transfermarkt.co.uk/spieler/vatersohnnationalspieler/statistik)).
Season-level stats like player transfers run back for quite a few seasons,
joyously arranged in predictable html tables for bountiful scraping. I wrote a
short scraping program[^scrape] to collect and clean up player transfers for
these (and other) leagues, back to the 1992/1993 season (N.B. this decision was
entirely arbitrary).

Et voila - the data is now freely available,
[hosted on Github as flat .csv files](https://github.com/ewenme/transfers/tree/master/data).

## Sketching some visuals on top

Now for a couple of visualisation pieces I've tried out using this dataset. I've
included some notes on my process/workflow for each[^viz], if you're that way
inclined. Otherwise, just _~absorb the inspiration~_ `r emo::ji("idea")`

I took a look at the value of player buys vs sales for Premier League clubs in
the 2018/19 window, using a
[Cleveland dot plot](https://uc-r.github.io/cleveland-dot-plots) (AKA 'dumbbell'
chart) variant.
![](https://github.com/ewenme/transfers/raw/master/figures/premier-league-transfer-spend-2018-web.png#c)

> This type of visualisation is an elegant and simple way to show ranges of data
> (i.e. spend vs sales difference) across multiple categories (i.e. football
> clubs). I did the initial sketch for this using my standard charting workflow
> in R (mostly [ggplot2](https://ggplot2.tidyverse.org/index.html) and it's many
> extensions, including [Bob Rudis'](https://rud.is/b/) charming
> [ggalt](https://github.com/hrbrmstr/ggalt) which made this chart type ), but I
> _did_ export this into
> [Adobe Illustrator](https://www.adobe.com/products/illustrator.html)
> ([Inkscape](https://inkscape.org/en/) is a fine free alternative) to do good
> text annotations quicker. The final version therefore includes
> non-reproducible elements that makes refreshing the viz for new transfer
> windows non-trivial, but that help in telling stories contained in this view
> of the data. In this one-off case, I think the trade-off is fine.

Next, a look at a single club's season-by-season transfer spend and sales,
following the relationship between these two amounts through time.

![](https://github.com/ewenme/transfers/raw/master/figures/chelsea-transfers-web.png#c)

> This visualisation choice might be a little difficult to follow at first, if
> it's your first connected scatter (in
> [this post by Elijah Meeks](https://medium.com/@Elijah_Meeks/what-charts-do-48ed96f70a74),
> the connected scatter example actually includes a link to an explanation of
> what's going on).
> [Steve Haroz + collaborators' research paper](http://steveharoz.com/research/connected_scatterplot/)
> was invaluable in guiding my first application of this chart format. In short,
> they are good at showing changing data for two variables whenever there is a
> relatively clear pattern of progression. Similarly to the previous example,
> this was sketched out in R with ggplot2, with some Illustrator annotation
> fine-tuning.

That's basically it...let me know if you make something 🔥

[^scrape]:
    For this post I chose to omit lengthy passages on web scraping, as not to
    deter non-programmers (insights can be gleaned from the cleaned data w/o
    additional code). However, the code used to scrape, clean and analyse the
    data is publicly available within the `src` directory of the
    [transfers GitHub repo](https://github.com/ewenme/transfers), featuring
    [rvest](https://github.com/hadley/rvest) (web scraping for R) in conjunction
    with [purrr](https://github.com/tidyverse/purrr) (iteration tools for R).

[^code]:
    You can find the R code used to generate this post
    [here](https://github.com/rbind/ewenme/blob/master/content/blog/2018-08-27-building-open-transfer-data.Rmd).

[^viz]:
    For the R code used to sketch the chart examples included in the post (and
    others that didn't make the cut), try
    [here](https://github.com/ewenme/transfers/blob/master/src/02-viz.R).
