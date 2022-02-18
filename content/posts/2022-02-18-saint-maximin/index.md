---
title: allan saint-maximin appreciation post
date: '2022-02-18'
tags: ['football']
keywords: ['football', 'r', 'rstats', 'newcastle united']
description: he dribbles when he wants.
slug: saint-max-appreciation
image: 'https://ewen.io/saint-max-appreciation/asm.jpeg'
draft: yes
output:
  hugodown::md_document:
    fig_asp: 0.8
rmd_hash: 26c771e9f84ae654
---

For my sins, I am a Newcastle United fan. Watching them play has been hard graft
for a decent while. Going deep in the UEFA Cup seems like a very quaint memory.
There's reason for renewed sporting optimism, with a helping of geopolitical
pessimism, now
[the club is owned by Saudi Arabia](https://www.bbc.co.uk/sport/football/58826899)
(for what it's worth, the previous owner Mike Ashley, has been
[regularly accused of paying staff below minimum wage](https://www.theguardian.com/business/2020/jul/23/sports-direct-undercover-minimum-wage-mike-ashley),
so the bar is unthinkably low).

These sport~~wash~~ing revolutions typically involve waves of new players
turning up to appease the new regime and expectations, inevitably taking places
off of (mostly) technically limited members of the squad first. Longer term,
this gentrification tends to be all-encompassing. I wanted to pay tribute to the
player that has managed to light up countless diabolical performances across
some forgettable seasons, while there's time left to enjoy more.

First, a chart to convey the relative pain that comes with watching the current
Newcastle team.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>nufc_xg</span> <span class='o'>&lt;-</span> <span class='nv'>nufc_matches</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://rdrr.io/pkg/janitor/man/clean_names.html'>clean_names</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'>dplyr</span><span class='nf'>::</span><span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
    <span class='nv'>comp</span> <span class='o'>==</span> <span class='s'>"Premier League"</span>, <span class='o'>!</span><span class='nf'><a href='https://rdrr.io/r/base/NA.html'>is.na</a></span><span class='o'>(</span><span class='nv'>x_g</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span><span class='o'>(</span>
    xg_rolling <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/pkg/slider/man/slide.html'>slide_dbl</a></span><span class='o'>(</span><span class='nv'>x_g</span>, <span class='nv'>mean</span>, .before <span class='o'>=</span> <span class='m'>6</span><span class='o'>)</span>,
    xga_rolling <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/pkg/slider/man/slide.html'>slide_dbl</a></span><span class='o'>(</span><span class='nv'>x_ga</span>, <span class='nv'>mean</span>, .before <span class='o'>=</span> <span class='m'>6</span><span class='o'>)</span>,
    xg_diff <span class='o'>=</span> <span class='nv'>xg_rolling</span> <span class='o'>-</span> <span class='nv'>xga_rolling</span>,
    gf <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/numeric.html'>as.numeric</a></span><span class='o'>(</span><span class='nv'>gf</span><span class='o'>)</span>,
    ga <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/numeric.html'>as.numeric</a></span><span class='o'>(</span><span class='nv'>ga</span><span class='o'>)</span>,
    g_rolling <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/pkg/slider/man/slide.html'>slide_dbl</a></span><span class='o'>(</span><span class='nv'>gf</span>, <span class='nv'>mean</span>, .before <span class='o'>=</span> <span class='m'>6</span><span class='o'>)</span>,
    ga_rolling <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/pkg/slider/man/slide.html'>slide_dbl</a></span><span class='o'>(</span><span class='nv'>ga</span>, <span class='nv'>mean</span>, .before <span class='o'>=</span> <span class='m'>6</span><span class='o'>)</span>,
    g_diff <span class='o'>=</span> <span class='nv'>g_rolling</span> <span class='o'>-</span> <span class='nv'>ga_rolling</span>,
    date <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/as.Date.html'>as.Date</a></span><span class='o'>(</span><span class='nv'>date</span><span class='o'>)</span>,
    gameweek <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/numeric.html'>as.numeric</a></span><span class='o'>(</span><span class='nf'><a href='https://stringr.tidyverse.org/reference/str_extract.html'>str_extract</a></span><span class='o'>(</span><span class='nv'>round</span>, <span class='s'>"[0-9]+"</span><span class='o'>)</span><span class='o'>)</span>,
    season <span class='o'>=</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/case_when.html'>case_when</a></span><span class='o'>(</span>
      <span class='nv'>date</span> <span class='o'>&lt;=</span> <span class='nf'><a href='https://rdrr.io/r/base/as.Date.html'>as.Date</a></span><span class='o'>(</span><span class='s'>"2021-05-23"</span><span class='o'>)</span> <span class='o'>~</span> <span class='s'>"2020-21"</span>,
      <span class='nv'>date</span> <span class='o'>&gt;=</span> <span class='nf'><a href='https://rdrr.io/r/base/as.Date.html'>as.Date</a></span><span class='o'>(</span><span class='s'>"2021-08-15"</span><span class='o'>)</span> <span class='o'>~</span> <span class='s'>"2021-22"</span>
    <span class='o'>)</span>
  <span class='o'>)</span>

<span class='nv'>manager_changes</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tribble.html'>tribble</a></span><span class='o'>(</span>
  <span class='o'>~</span><span class='nv'>gameweek</span>, <span class='o'>~</span><span class='nv'>season</span>,
  <span class='m'>9</span>, <span class='s'>"2021-22"</span>,
  <span class='m'>11</span>, <span class='s'>"2021-22"</span>
<span class='o'>)</span>

<span class='nv'>managers</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tribble.html'>tribble</a></span><span class='o'>(</span>
  <span class='o'>~</span><span class='nv'>name</span>, <span class='o'>~</span><span class='nv'>gameweek</span>, <span class='o'>~</span><span class='nv'>season</span>, <span class='o'>~</span><span class='nv'>angle</span>, <span class='o'>~</span><span class='nv'>y</span>,
  <span class='s'>"Steve Bruce"</span>, <span class='m'>5</span>, <span class='s'>"2021-22"</span>, <span class='m'>0</span>, <span class='m'>1.5</span>,
  <span class='s'>"Graeme Jones"</span>, <span class='m'>10</span>, <span class='s'>"2021-22"</span>, <span class='m'>90</span>, <span class='m'>1.8</span>,
  <span class='s'>"Eddie Howe"</span>, <span class='m'>18</span>, <span class='s'>"2021-22"</span>, <span class='m'>0</span>, <span class='m'>1.5</span>
<span class='o'>)</span>

<span class='nv'>series_labels</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tibble.html'>tibble</a></span><span class='o'>(</span>
  name <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"xG difference"</span>, <span class='s'>"goal difference"</span><span class='o'>)</span>,
  colour <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>asm_col</span>, <span class='nv'>highlight_col</span><span class='o'>)</span>,
  gameweek <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='m'>15</span>, <span class='m'>5</span><span class='o'>)</span>,
  y <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='o'>-</span><span class='m'>.25</span>, <span class='m'>.25</span><span class='o'>)</span>,
  season <span class='o'>=</span> <span class='s'>"2020-21"</span>
<span class='o'>)</span>

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span>
  <span class='nv'>nufc_xg</span>,
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nv'>gameweek</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/facet_wrap.html'>facet_wrap</a></span><span class='o'>(</span><span class='o'>~</span><span class='nv'>season</span>, scales <span class='o'>=</span> <span class='s'>"free_x"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># add the series</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_ribbon.html'>geom_ribbon</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>ymin <span class='o'>=</span> <span class='nv'>xg_diff</span>, ymax <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/Extremes.html'>pmin</a></span><span class='o'>(</span><span class='nv'>xg_diff</span>, <span class='nv'>g_diff</span><span class='o'>)</span><span class='o'>)</span>,
    alpha <span class='o'>=</span> <span class='m'>0.3</span>, fill <span class='o'>=</span> <span class='nv'>asm_col</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_ribbon.html'>geom_ribbon</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>ymin <span class='o'>=</span> <span class='nv'>g_diff</span>, ymax <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/Extremes.html'>pmin</a></span><span class='o'>(</span><span class='nv'>xg_diff</span>, <span class='nv'>g_diff</span><span class='o'>)</span><span class='o'>)</span>,
    alpha <span class='o'>=</span> <span class='m'>0.3</span>, fill <span class='o'>=</span> <span class='nv'>highlight_col</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_path.html'>geom_line</a></span><span class='o'>(</span><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>y <span class='o'>=</span> <span class='nv'>xg_diff</span>, colour <span class='o'>=</span> <span class='s'>"xG difference"</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_path.html'>geom_line</a></span><span class='o'>(</span><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>y <span class='o'>=</span> <span class='nv'>g_diff</span>, colour <span class='o'>=</span> <span class='s'>"goal difference"</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># add reference lines + text</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_abline.html'>geom_hline</a></span><span class='o'>(</span>yintercept <span class='o'>=</span> <span class='m'>0</span>, linetype <span class='o'>=</span> <span class='s'>"dashed"</span>, colour <span class='o'>=</span> <span class='nv'>unhighlighted_col</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_abline.html'>geom_vline</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>xintercept <span class='o'>=</span> <span class='nv'>gameweek</span><span class='o'>)</span>, data <span class='o'>=</span> <span class='nv'>manager_changes</span>,
    linetype <span class='o'>=</span> <span class='s'>"dashed"</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_text.html'>geom_text</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>name</span>, angle <span class='o'>=</span> <span class='nv'>angle</span>, y <span class='o'>=</span> <span class='nv'>y</span><span class='o'>)</span>, data <span class='o'>=</span> <span class='nv'>managers</span>,
    hjust <span class='o'>=</span> <span class='m'>1</span>, family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>, fontface <span class='o'>=</span> <span class='s'>"bold"</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_text.html'>geom_text</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>name</span>, y <span class='o'>=</span> <span class='nv'>y</span><span class='o'>)</span>, data <span class='o'>=</span> <span class='nv'>series_labels</span>, hjust <span class='o'>=</span> <span class='m'>0</span>,
    family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>, fontface <span class='o'>=</span> <span class='s'>"bold"</span>,
    colour <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>asm_col</span>, <span class='nv'>highlight_col</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># theme stuff</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_manual.html'>scale_color_manual</a></span><span class='o'>(</span>
    values <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>highlight_col</span>, <span class='nv'>asm_col</span><span class='o'>)</span>, name <span class='o'>=</span> <span class='kc'>NULL</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>
    title <span class='o'>=</span> <span class='s'>"This season, Newcastle have &lt;span style = 'color:#ef233c;'&gt;under-performed&lt;/span&gt; relative to an already poor xG difference"</span>,
    subtitle <span class='o'>=</span> <span class='s'>"6 game rolling goal and xG difference | English Premier League"</span>,
    x <span class='o'>=</span> <span class='s'>"gameweek"</span>,
    y <span class='o'>=</span> <span class='s'>"goal/xG difference"</span>,
    caption <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Players with at least 900 minutes played.
      Source: FBref, &#123;today&#125;
      @ewen_"</span>
    <span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/pilot/man/theme_pilot.html'>theme_pilot</a></span><span class='o'>(</span>grid <span class='o'>=</span> <span class='s'>"h"</span>, legend_position <span class='o'>=</span> <span class='s'>"none"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span><span class='o'>(</span>
    plot.title <span class='o'>=</span> <span class='nf'><a href='https://wilkelab.org/ggtext/reference/element_textbox.html'>element_textbox</a></span><span class='o'>(</span>
      size <span class='o'>=</span> <span class='m'>18</span>, padding <span class='o'>=</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>margin</a></span><span class='o'>(</span><span class='m'>0</span>, <span class='m'>0</span>, <span class='m'>5</span>, <span class='m'>0</span><span class='o'>)</span>,
      hjust <span class='o'>=</span> <span class='m'>0</span>
    <span class='o'>)</span>,
    plot.title.position <span class='o'>=</span> <span class='s'>"plot"</span>
  <span class='o'>)</span>
</code></pre>
<img src="figs/nufc-xg-1.png" width="100%" style="display: block; margin: auto;" />

</div>

Not only has the xG difference been mostly below zero for two consecutive
seasons (i.e. no. of goals Newcastle should have scored, given the quality of
chances created, minus the no. of goals their opposition should have scored),
their actual goal difference has mostly lagged behind this as well. This is
characteristic of a low-quality team.

So, why bother? Even in the darkest days of Sports Direct FC, there have been
glimmers of joy to grab with both hands. Once upon a time, Alan Pardew might
have treated us with a Hatem Ben Arfa cameo. Today, we are pacified by the
one-man dribbling band that is Allan Saint-Maximin.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>pl_possession</span> <span class='o'>&lt;-</span> <span class='nv'>big5_possession</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://rdrr.io/pkg/janitor/man/clean_names.html'>clean_names</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'>dplyr</span><span class='nf'>::</span><span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
    <span class='nv'>comp</span> <span class='o'>==</span> <span class='s'>"Premier League"</span>, <span class='nv'>mins_per_90</span> <span class='o'>&gt;=</span> <span class='m'>10</span>
  <span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span><span class='o'>(</span>
    succ_dribbles_90 <span class='o'>=</span> <span class='nv'>succ_dribbles</span> <span class='o'>/</span> <span class='nv'>mins_per_90</span>,
    att_dribbles_90 <span class='o'>=</span> <span class='nv'>att_dribbles</span> <span class='o'>/</span> <span class='nv'>mins_per_90</span>,
    prog_carries_90 <span class='o'>=</span> <span class='nv'>prog_carries</span> <span class='o'>/</span> <span class='nv'>mins_per_90</span>,
    is_amn <span class='o'>=</span> <span class='nv'>player</span> <span class='o'>==</span> <span class='s'>"Allan Saint-Maximin"</span>
  <span class='o'>)</span>

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span>
  <span class='nv'>pl_possession</span>,
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nv'>succ_dribbles_90</span>, y <span class='o'>=</span> <span class='nv'>prog_carries_90</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span><span class='o'>(</span>
    shape <span class='o'>=</span> <span class='m'>21</span>, alpha <span class='o'>=</span> <span class='m'>.9</span>, size <span class='o'>=</span> <span class='m'>3</span>,
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>fill <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>factor</a></span><span class='o'>(</span><span class='nv'>is_amn</span><span class='o'>)</span>, colour <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>factor</a></span><span class='o'>(</span><span class='nv'>is_amn</span><span class='o'>)</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/gghighlight/man/gghighlight.html'>gghighlight</a></span><span class='o'>(</span>
    <span class='nv'>succ_dribbles_90</span> <span class='o'>&gt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>succ_dribbles_90</span>, <span class='m'>0.95</span><span class='o'>)</span> <span class='o'>|</span>
      <span class='nv'>prog_carries_90</span> <span class='o'>&gt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>prog_carries_90</span>, <span class='m'>0.95</span><span class='o'>)</span>,
    unhighlighted_params <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='o'>(</span>
      colour <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>, fill <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>, alpha <span class='o'>=</span> <span class='m'>0.3</span>
    <span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/ggrepel/man/geom_text_repel.html'>geom_text_repel</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>player</span><span class='o'>)</span>, colour <span class='o'>=</span> <span class='nv'>highlight_col</span>,
    data <span class='o'>=</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
      <span class='nv'>pl_possession</span>,
      <span class='nv'>succ_dribbles_90</span> <span class='o'>&gt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>succ_dribbles_90</span>, <span class='m'>0.95</span><span class='o'>)</span> <span class='o'>|</span>
        <span class='nv'>prog_carries_90</span> <span class='o'>&gt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>prog_carries_90</span>, <span class='m'>0.95</span><span class='o'>)</span>
    <span class='o'>)</span>,
    size <span class='o'>=</span> <span class='m'>3.5</span>, family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_manual.html'>scale_color_manual</a></span><span class='o'>(</span>values <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>highlight_col</span>, <span class='nv'>asm_col</span><span class='o'>)</span>, guide <span class='o'>=</span> <span class='s'>"none"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_manual.html'>scale_fill_manual</a></span><span class='o'>(</span>values <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>highlight_col</span>, <span class='nv'>asm_col</span><span class='o'>)</span>, guide <span class='o'>=</span> <span class='s'>"none"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>
    title <span class='o'>=</span> <span class='s'>"&lt;span style = 'color:#ef233c;'&gt;Saint-Maximin's&lt;/span&gt; volume dribbling is in Adama Traoré territory"</span>,
    subtitle <span class='o'>=</span> <span class='s'>"Successful dribbles and carries per 90 minutes | English Premier League 2021-22"</span>,
    x <span class='o'>=</span> <span class='s'>"dribbles completed per 90"</span>,
    y <span class='o'>=</span> <span class='s'>"progressive carries per 90"</span>,
    caption <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Players with at least 900 minutes played.
      Source: FBref, &#123;today&#125;
      @ewen_"</span>
      <span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/pilot/man/theme_pilot.html'>theme_pilot</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span><span class='o'>(</span>
    plot.title <span class='o'>=</span> <span class='nf'><a href='https://wilkelab.org/ggtext/reference/element_textbox.html'>element_textbox</a></span><span class='o'>(</span>
      size <span class='o'>=</span> <span class='m'>18</span>, padding <span class='o'>=</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>margin</a></span><span class='o'>(</span><span class='m'>0</span>, <span class='m'>0</span>, <span class='m'>5</span>, <span class='m'>0</span><span class='o'>)</span>,
      hjust <span class='o'>=</span> <span class='m'>0</span>
    <span class='o'>)</span>,
    plot.title.position <span class='o'>=</span> <span class='s'>"plot"</span>
  <span class='o'>)</span>
</code></pre>
<img src="figs/dribble-carries-1.png" width="100%" style="display: block; margin: auto;" />

</div>

Given that Adama has now left the league, if you want to watch a jinking winger
there is literally no competition. This is compounded by the fact that he plays
for Newcastle, where the next most dribble-happy teammate is a way off. The
"second guy" is further away than any other PL team.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>pl_possession</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span><span class='o'>(</span><span class='nv'>squad</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice_max</a></span><span class='o'>(</span><span class='nv'>att_dribbles_90</span>, n <span class='o'>=</span> <span class='m'>2</span>, with_ties <span class='o'>=</span> <span class='kc'>FALSE</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span><span class='o'>(</span>
    dribble_diff <span class='o'>=</span> <span class='nv'>att_dribbles_90</span> <span class='o'>-</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/lead-lag.html'>lag</a></span><span class='o'>(</span><span class='nv'>att_dribbles_90</span><span class='o'>)</span>,
    top_dribbles_90 <span class='o'>=</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/lead-lag.html'>lag</a></span><span class='o'>(</span><span class='nv'>att_dribbles_90</span><span class='o'>)</span>,
    top_player <span class='o'>=</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/lead-lag.html'>lag</a></span><span class='o'>(</span><span class='nv'>player</span><span class='o'>)</span>
    <span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'>dplyr</span><span class='nf'>::</span><span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span><span class='o'>!</span><span class='nf'><a href='https://rdrr.io/r/base/NA.html'>is.na</a></span><span class='o'>(</span><span class='nv'>dribble_diff</span><span class='o'>)</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span><span class='o'>(</span>
    <span class='nv'>squad</span>, <span class='nv'>top_player</span>, <span class='nv'>player</span>, <span class='nv'>top_dribbles_90</span>,
    <span class='nv'>att_dribbles_90</span>, <span class='nv'>dribble_diff</span>
    <span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>
      y <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/stats/reorder.factor.html'>reorder</a></span><span class='o'>(</span><span class='nv'>squad</span>, <span class='o'>-</span><span class='nv'>dribble_diff</span><span class='o'>)</span>
    <span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_linerange.html'>geom_linerange</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>xmin <span class='o'>=</span> <span class='nv'>att_dribbles_90</span>, xmax <span class='o'>=</span> <span class='nv'>top_dribbles_90</span><span class='o'>)</span>,
    colour <span class='o'>=</span> <span class='nv'>highlight_col</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nv'>att_dribbles_90</span><span class='o'>)</span>,
    shape <span class='o'>=</span> <span class='m'>21</span>, size <span class='o'>=</span> <span class='m'>4</span>, colour <span class='o'>=</span> <span class='nv'>highlight_col</span>, fill <span class='o'>=</span> <span class='s'>"#ffffff"</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nv'>top_dribbles_90</span><span class='o'>)</span>,
    shape <span class='o'>=</span> <span class='m'>21</span>, size <span class='o'>=</span> <span class='m'>4</span>, colour <span class='o'>=</span> <span class='nv'>highlight_col</span>, fill <span class='o'>=</span> <span class='s'>"#ffffff"</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_text.html'>geom_text</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>player</span>, x <span class='o'>=</span> <span class='nv'>att_dribbles_90</span><span class='o'>)</span>,
    hjust <span class='o'>=</span> <span class='m'>1</span>, nudge_x <span class='o'>=</span> <span class='o'>-</span><span class='m'>.1</span>, size <span class='o'>=</span> <span class='m'>3.5</span>,
    family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>, colour <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_text.html'>geom_text</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>top_player</span>, x <span class='o'>=</span> <span class='nv'>top_dribbles_90</span><span class='o'>)</span>,
    hjust <span class='o'>=</span> <span class='m'>0</span>, nudge_x <span class='o'>=</span> <span class='m'>.1</span>, size <span class='o'>=</span> <span class='m'>3.5</span>,
    family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>, colour <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_continuous.html'>scale_x_continuous</a></span><span class='o'>(</span>limits <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='m'>0</span>, <span class='m'>10</span><span class='o'>)</span>, expand <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='m'>0</span>, <span class='kc'>NA</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/pilot/man/theme_pilot.html'>theme_pilot</a></span><span class='o'>(</span>grid <span class='o'>=</span> <span class='s'>"v"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>
    title <span class='o'>=</span> <span class='s'>"Saint-Maximin's dribble attempts dwarf the club's #2 dribbler, Joe Willock"</span>,
    subtitle <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Top two players per club for attempted dribbles per 90 minutes | English Premier League 2021-22"</span>
      <span class='o'>)</span>,
    x <span class='o'>=</span> <span class='s'>"dribbles attempted per 90"</span>,
    y <span class='o'>=</span> <span class='kc'>NULL</span>,
    caption <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Players with at least 900 minutes played.
      Source: FBref, &#123;today&#125;
      @ewen_"</span>
    <span class='o'>)</span>
  <span class='o'>)</span>
</code></pre>
<img src="figs/top-club-dribblers-1.png" width="640px" style="display: block; margin: auto;" />

</div>

How does he do it? Well, it turns out that Allan doesn't really do much of
anything out of possession. There's only one fella that pressures the opposition
less.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>pl_def</span> <span class='o'>&lt;-</span> <span class='nv'>big5_def</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://rdrr.io/pkg/janitor/man/clean_names.html'>clean_names</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'>dplyr</span><span class='nf'>::</span><span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
    <span class='nv'>comp</span> <span class='o'>==</span> <span class='s'>"Premier League"</span>, <span class='nv'>mins_per_90</span> <span class='o'>&gt;=</span> <span class='m'>10</span>,
    <span class='nv'>pos</span> <span class='o'><a href='https://rdrr.io/r/base/match.html'>%in%</a></span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"FW"</span>, <span class='s'>"FW,MF"</span>, <span class='s'>"MF,FW"</span>, <span class='s'>"MF"</span><span class='o'>)</span>,
  <span class='o'>)</span> <span class='o'><a href='https://rdrr.io/pkg/janitor/man/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span><span class='o'>(</span>
    tackles_90 <span class='o'>=</span> <span class='nv'>tkl_tackles</span> <span class='o'>/</span> <span class='nv'>mins_per_90</span>,
    pressures_90 <span class='o'>=</span> <span class='nv'>press_pressures</span> <span class='o'>/</span> <span class='nv'>mins_per_90</span>,
    is_amn <span class='o'>=</span> <span class='nv'>player</span> <span class='o'>==</span> <span class='s'>"Allan Saint-Maximin"</span>
  <span class='o'>)</span>

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span>
  <span class='nv'>pl_def</span>, <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nv'>tackles_90</span>, y <span class='o'>=</span> <span class='nv'>pressures_90</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span><span class='o'>(</span>
    shape <span class='o'>=</span> <span class='m'>21</span>, alpha <span class='o'>=</span> <span class='m'>.7</span>, size <span class='o'>=</span> <span class='m'>3</span>,
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>fill <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>factor</a></span><span class='o'>(</span><span class='nv'>is_amn</span><span class='o'>)</span>, colour <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/factor.html'>factor</a></span><span class='o'>(</span><span class='nv'>is_amn</span><span class='o'>)</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/gghighlight/man/gghighlight.html'>gghighlight</a></span><span class='o'>(</span>
    <span class='nv'>squad</span> <span class='o'>==</span> <span class='s'>"Newcastle Utd"</span>,
    unhighlighted_params <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='o'>(</span>
      colour <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>, fill <span class='o'>=</span> <span class='nv'>unhighlighted_col</span>,
      alpha <span class='o'>=</span> <span class='m'>0.3</span>
      <span class='o'>)</span>
    <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/ggrepel/man/geom_text_repel.html'>geom_text_repel</a></span><span class='o'>(</span>
    <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label <span class='o'>=</span> <span class='nv'>player</span><span class='o'>)</span>,
    data <span class='o'>=</span> <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
      <span class='nv'>pl_def</span>,
      <span class='o'>(</span>
        <span class='nv'>tackles_90</span> <span class='o'>&lt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>tackles_90</span>, <span class='m'>0.01</span><span class='o'>)</span> <span class='o'>|</span>
          <span class='nv'>pressures_90</span> <span class='o'>&lt;</span> <span class='nf'><a href='https://rdrr.io/r/stats/quantile.html'>quantile</a></span><span class='o'>(</span><span class='nv'>pressures_90</span>, <span class='m'>0.01</span><span class='o'>)</span>
      <span class='o'>)</span> <span class='o'>|</span> <span class='nv'>squad</span> <span class='o'>==</span> <span class='s'>"Newcastle Utd"</span>
    <span class='o'>)</span>,
    box.padding <span class='o'>=</span> <span class='m'>0.5</span>, colour <span class='o'>=</span> <span class='nv'>highlight_col</span>,
    family <span class='o'>=</span> <span class='s'>"iA Writer Quattro S"</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_manual.html'>scale_color_manual</a></span><span class='o'>(</span>values <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>highlight_col</span>, <span class='nv'>asm_col</span><span class='o'>)</span>, guide <span class='o'>=</span> <span class='s'>"none"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/scale_manual.html'>scale_fill_manual</a></span><span class='o'>(</span>values <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='nv'>highlight_col</span>, <span class='nv'>asm_col</span><span class='o'>)</span>, guide <span class='o'>=</span> <span class='s'>"none"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>
    title <span class='o'>=</span> <span class='s'>"Only Ronaldo pressures less than &lt;span style = 'color:#ef233c;'&gt;Saint-Maximin&lt;/span&gt; in the Premier League"</span>,
    subtitle <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Tackles and pressures applied per 90 minutes | English Premier League, 2021-22"</span>
    <span class='o'>)</span>,
    x <span class='o'>=</span> <span class='s'>"tackles per 90"</span>,
    y <span class='o'>=</span> <span class='s'>"pressures per 90"</span>,
    caption <span class='o'>=</span> <span class='nf'><a href='https://glue.tidyverse.org/reference/glue.html'>glue</a></span><span class='o'>(</span>
      <span class='s'>"Players with at least 900 minutes played.
      Source: FBref, &#123;today&#125;
      @ewen_"</span>
    <span class='o'>)</span>
  <span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/pilot/man/theme_pilot.html'>theme_pilot</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span><span class='o'>(</span>
    plot.title <span class='o'>=</span> <span class='nf'><a href='https://wilkelab.org/ggtext/reference/element_textbox.html'>element_textbox</a></span><span class='o'>(</span>
      size <span class='o'>=</span> <span class='m'>18</span>, padding <span class='o'>=</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>margin</a></span><span class='o'>(</span><span class='m'>0</span>, <span class='m'>0</span>, <span class='m'>5</span>, <span class='m'>0</span><span class='o'>)</span>,
      hjust <span class='o'>=</span> <span class='m'>0</span>
    <span class='o'>)</span>,
    plot.title.position <span class='o'>=</span> <span class='s'>"plot"</span>
  <span class='o'>)</span>
</code></pre>
<img src="figs/pressures-1.png" width="640px" style="display: block; margin: auto;" />

</div>

tired: ASM is preserved for when Newcastle are in possession and need him to get
the team up the field.

inspired: ASM doesn't need to defend because he is doing up defences like this.
Never change.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'>blogdown</span><span class='nf'>::</span><span class='nf'><a href='https://pkgs.rstudio.com/blogdown/reference/shortcode.html'>shortcode</a></span><span class='o'>(</span><span class='s'>"youtube"</span>, <span class='s'>"dI6wSTgM9Zs"</span><span class='o'>)</span>
</code></pre>

{{% youtube "dI6wSTgM9Zs" %}}

</div>
