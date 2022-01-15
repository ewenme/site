---
title: understatr
date: '2018-12-10'
tags: ['football', 'r']
keywords: ['understat', 'understatr', 'football', 'r']
description:
  An understated (*cough*) project to help folks get hold of tidy football data.
slug: understatr
image: '/blog/2018-12-10-understatr_files/figure-html/dortmund-plot-1.png'
draft: no
---

[understat](https://understat.com/) is a treasure trove of football statistics
and advanced metrics. All of your ~~"old man yells at cloud" conjuring~~
household favourites are here. Democratized access to xG for casuals everywhere,
so we can do unholy things like this.

{{< rawhtml >}}

<blockquote class="twitter-tweet" data-lang="en"><p lang="und" dir="ltr"><a href="https://t.co/cNiXEsfAms">pic.twitter.com/cNiXEsfAms</a></p>&mdash; Ryan O&#39;Hanlon (@rwohan) <a href="https://twitter.com/rwohan/status/1071159056096579585?ref_src=twsrc%5Etfw">December 7, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
{{< /rawhtml >}}

While the site itself is easy to navigate and features nice off-the-shelf
exploratory viz tools (à la the above radar), many (myself included) can get a
bit fidgety without access to all this stuff in a structured format for
analysis.

So, I made a thing to grab understat data and tidy it up, so you can get on with
the interesting stuff. Say 👋 to
[understatr](https://github.com/ewenme/understatr), an
[R](https://www.r-project.org/) package that makes pulling xG numbers as easy as
🥧

```r
# load package
library(understatr)

# get dortmund playing squad data for this season
dortmund <- get_team_players_stats(team_name = "Borussia Dortmund", year = 2018)

dortmund
```

```
## # A tibble: 23 × 19
##    player_id player_name  games  time goals    xG assists    xA shots key_passes
##        <dbl> <chr>        <dbl> <dbl> <dbl> <dbl>   <dbl> <dbl> <dbl>      <dbl>
##  1      2380 Paco Alcácer    26  1181    18 10.2        0  1.61    52         15
##  2       319 Marco Reus      27  2335    17 16.6        8  6.94    69         38
##  3      6345 Jadon Sancho    34  2463    12  7.42      14  8.31    40         63
##  4       422 Mario Götze     26  1694     7  4.84       7  6.64    28         41
##  5      2662 Christian P…    20   916     4  5.16       4  2.97    27         21
##  6      3059 Axel Witsel     33  2873     4  2.88       1  2.03    36         16
##  7      5230 Thomas Dela…    30  2302     3  2.76       5  2.90    33         15
##  8      3455 Raphael Gue…    23  1539     2  2.88       6  3.94    35         38
##  9      5197 Achraf Haki…    21  1740     2  1.69       4  2.96    20         11
## 10      5355 Jacob Bruun…    24  1267     2  3.27       2  3.16    26         17
## # … with 13 more rows, and 9 more variables: yellow_cards <dbl>,
## #   red_cards <dbl>, position <chr>, team_name <chr>, npg <dbl>, npxG <dbl>,
## #   xGChain <dbl>, xGBuildup <dbl>, year <dbl>
```

You can use this in conjunction with the [tidyverse](https://www.tidyverse.org/)
to get from data to viz lighting-quick.

```r
library(tidyverse)
library(ewenthemes) # (a personal chart themes pkg)

dortmund %>%
  # remove players w/ zero xG
  filter(xG > 0) %>%
  # chart xG in desc order
  ggplot(aes(x = reorder(player_name, xG), y = xG)) +
  # make it a bar chart
  geom_col() +
  # flip the bars
  coord_flip() +
  # add some labels (making sure to credit understat!)
  labs(title = "Expected goals contributions", subtitle = "Borussia Dortmund, 2018/19",
       x = NULL, caption = "source: understat.com | @ewen_") +
  # add my personal chart theme
  theme_ewen_rs(grid = "X", axis = FALSE, axis_text_size = 9)
```

![](index_files/figure-html/dortmund-plot-1.svg)

Peep the project's GitHub page for updates, for now at least. As noted in the
[readme](https://github.com/ewenme/understatr/blob/master/README.md), while
non-commercial use of the data is fine for now, I don't own it and similarly
don't control changes to the website. I say this to say that `understatr` may
break (or become illegal) in future, so enjoy it while you can 🎈 I'd be psyched
to hear about people using it, or even contributing to it. Esp. here for the
hottest of hot take Messi radar trolls, please cite the proj in all of those.
