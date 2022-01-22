---
title: 'discogger (day one)'
description: 'Building an R interface to the Discogs API.'
date: '2018-06-07'
slug: discogger-day-one
tags: ['music', 'r']
keywords: ['discogs', 'discogger', 'r', 'api']
image: https://raw.githubusercontent.com/rbind/ewenme/master/static/blog/2018-06-07-discogger-day-one_files/figure-html/unnamed-chunk-4-1.png
draft: no
output:
  hugodown::md_document:
    fig_asp: 0.8
rmd_hash: ade6cc534325bc23
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
<https://www.discogs.com/settings/developers>. Doing so will grant you a
"personal access token" (accessible at the same link above) which you can store
as an environment variable (_DISCOGS_API_TOKEN_) using the
[`discogs_api_token()`](https://ewenme.github.io/discogger/reference/discogs_api_token.html)
function. All functions in the `discogger` package will automatically look for
your token and prompt entry if it isn't found.

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
information, with
[`discogs_label()`](https://ewenme.github.io/discogger/reference/discogs_label.html)
first up.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># load packages</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://ewenme.github.io/discogger'>discogger</a></span><span class='o'>)</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://tidyverse.tidyverse.org'>tidyverse</a></span><span class='o'>)</span>

<span class='c'># get DM label info</span>
<span class='nf'><a href='https://ewenme.github.io/discogger/reference/discogs_label.html'>discogs_label</a></span><span class='o'>(</span>label_id <span class='o'>=</span> <span class='m'>314</span><span class='o'>)</span>
<span class='c'>#&gt; &lt;Discogs labels/314&gt;</span>
<span class='c'>#&gt; Preview: 1 of 12 results. </span>
<span class='c'>#&gt; List of 1</span>
<span class='c'>#&gt;  $ id: int 314</span></code></pre>

</div>

Hopefully you learnt something new from that output, but it's not particularly
ready for any analysis. I'm quite interested in getting more detailed metadata
around Dance Mania's 12\" releases, which can be done with
[`discogs_label_releases()`](https://ewenme.github.io/discogger/reference/discogs_label_releases.html).

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># get DM releases</span>
<span class='nv'>dm_releases</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://ewenme.github.io/discogger/reference/discogs_label_releases.html'>discogs_label_releases</a></span><span class='o'>(</span>label_id <span class='o'>=</span> <span class='m'>314</span><span class='o'>)</span>
  
<span class='c'># extract request content</span>
<span class='nv'>dm_releases_df</span> <span class='o'>&lt;-</span> <span class='nv'>dm_releases</span><span class='o'>$</span><span class='nv'>content</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span>
  <span class='c'># return 12" releases only</span>
  <span class='nf'>dplyr</span><span class='nf'>::</span><span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span><span class='nf'><a href='https://rdrr.io/r/base/grep.html'>grepl</a></span><span class='o'>(</span><span class='s'>'12"'</span>, <span class='nv'>format</span><span class='o'>)</span><span class='o'>)</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span> 
  <span class='nf'><a href='https://tibble.tidyverse.org/reference/as_tibble.html'>as_tibble</a></span><span class='o'>(</span><span class='o'>)</span>

<span class='nv'>dm_releases_df</span>
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 599 × 14</span></span>
<span class='c'>#&gt;    status   format     catno  thumb    resource_url  title     id  year artist  </span>
<span class='c'>#&gt;    <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>    <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>      <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>  <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>    <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>         <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>  <span style='color: #555555; font-style: italic;'>&lt;int&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;int&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>   </span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     14040  https:/… https://api.… Hous… 6.40<span style='color: #555555;'>e</span>4  <span style='text-decoration: underline;'>1</span>987 The Hou…</span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     51002… https:/… https://api.… What… 1.09<span style='color: #555555;'>e</span>6  <span style='text-decoration: underline;'>1</span>985 The Bro…</span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     B.C. … https:/… https://api.… Hous… 4.21<span style='color: #555555;'>e</span>5  <span style='text-decoration: underline;'>1</span>987 The Hou…</span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     B.C. … https:/… https://api.… Hous… 1.97<span style='color: #555555;'>e</span>5  <span style='text-decoration: underline;'>1</span>987 The Hou…</span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     BASIC… https:/… https://api.… Akce… 3.64<span style='color: #555555;'>e</span>4  <span style='text-decoration: underline;'>1</span>998 DJ Deeon</span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     D.J. … https:/… https://api.… Frea… 6.75<span style='color: #555555;'>e</span>4  <span style='text-decoration: underline;'>1</span>996 D.J. Fu…</span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span> Accepted <span style='color: #555555;'>"</span>12\", TP<span style='color: #555555;'>"</span> D.M. … https:/… https://api.… This… 5.17<span style='color: #555555;'>e</span>6  <span style='text-decoration: underline;'>1</span>988 Mello D.</span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     D.M. … https:/… https://api.… This… 4.27<span style='color: #555555;'>e</span>5  <span style='text-decoration: underline;'>1</span>988 Mello D.</span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     DM 003 https:/… https://api.… Hous… 7.30<span style='color: #555555;'>e</span>3  <span style='text-decoration: underline;'>1</span>986 The Hou…</span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span> Accepted <span style='color: #555555;'>"</span>12\"<span style='color: #555555;'>"</span>     DM 004 https:/… https://api.… Hard… 1.4 <span style='color: #555555;'>e</span>3  <span style='text-decoration: underline;'>1</span>987 Duane &amp;…</span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 589 more rows, and 5 more variables:</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   stats.community.in_wantlist &lt;int&gt;, stats.community.in_collection &lt;int&gt;,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   stats.user.in_wantlist &lt;int&gt;, stats.user.in_collection &lt;int&gt;,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   label_id &lt;dbl&gt;</span></span></code></pre>

</div>

The tidy tibble format returned is much better suited to analysis in R, but the
content I'm after is not quite there. Turns out community metrics, such as the
number of users who own or have marked a record as one they "want", are
available from a records release page (not it's label release listing).
[`discogs_release()`](https://ewenme.github.io/discogger/reference/discogs_release.html)
can be used, in conjunction with
[`map()`](https://purrr.tidyverse.org/reference/map.html) (from the
[`purrr`](https://purrr.tidyverse.org/) package), to iterate through releases
and collect this information.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># get DM release info</span>
<span class='nv'>dm_releases_info</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map</a></span><span class='o'>(</span><span class='nv'>dm_releases_df</span><span class='o'>$</span><span class='nv'>id</span>, <span class='nv'>discogs_release</span><span class='o'>)</span>

<span class='c'># extract release content</span>
<span class='nv'>dm_releases_content</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map</a></span><span class='o'>(</span><span class='nv'>dm_releases_info</span>, <span class='s'>"content"</span><span class='o'>)</span>
  
<span class='c'># extract fields relevant for community metrics analysis</span>
<span class='nv'>dm_community_metrics</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tibble.html'>tibble</a></span><span class='o'>(</span>
  title <span class='o'>=</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map_chr</a></span><span class='o'>(</span><span class='nv'>dm_releases_content</span>, <span class='s'>"title"</span><span class='o'>)</span>,
  artist <span class='o'>=</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map_chr</a></span><span class='o'>(</span><span class='nv'>dm_releases_content</span>, <span class='s'>"artists_sort"</span><span class='o'>)</span>,
  year <span class='o'>=</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map_int</a></span><span class='o'>(</span><span class='nv'>dm_releases_content</span>, <span class='s'>"year"</span><span class='o'>)</span>,
  have <span class='o'>=</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map_int</a></span><span class='o'>(</span><span class='nv'>dm_releases_content</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"community"</span>, <span class='s'>"have"</span><span class='o'>)</span><span class='o'>)</span>,
  want <span class='o'>=</span> <span class='nf'><a href='https://purrr.tidyverse.org/reference/map.html'>map_int</a></span><span class='o'>(</span><span class='nv'>dm_releases_content</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"community"</span>, <span class='s'>"want"</span><span class='o'>)</span><span class='o'>)</span>
  <span class='o'>)</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span>
  <span class='c'># for each record...</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span><span class='o'>(</span><span class='nv'>title</span>, <span class='nv'>artist</span><span class='o'>)</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span>
    <span class='c'># keep the one most users have...</span>
    <span class='nv'>have</span> <span class='o'>==</span> <span class='nf'><a href='https://rdrr.io/r/base/Extremes.html'>max</a></span><span class='o'>(</span><span class='nv'>have</span><span class='o'>)</span>,
    <span class='c'># ...and rm releases with missing release year</span>
    <span class='nv'>year</span> <span class='o'>&gt;=</span> <span class='m'>1985</span>
    <span class='o'>)</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span> 
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span><span class='o'>(</span><span class='o'>)</span>

<span class='nv'>dm_community_metrics</span>
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 313 × 5</span></span>
<span class='c'>#&gt;    title                      artist                            year  have  want</span>
<span class='c'>#&gt;    <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>                      <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>                            <span style='color: #555555; font-style: italic;'>&lt;int&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;int&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span> What's That                Browns, The                       <span style='text-decoration: underline;'>1</span>985    86   276</span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span> Akceier 8                  DJ Deeon                          <span style='text-decoration: underline;'>1</span>998   214    82</span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span> Freaky Style Take: 2       DJ Funk                           <span style='text-decoration: underline;'>1</span>996   221   257</span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span> This X-Mas Rap             Mello D.                          <span style='text-decoration: underline;'>1</span>988     3    36</span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span> This Christmas (Rap)       Mello D.                          <span style='text-decoration: underline;'>1</span>988    41    98</span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span> House Nation               Housemaster Boyz, The And Rude …  <span style='text-decoration: underline;'>1</span>986  <span style='text-decoration: underline;'>1</span>381  <span style='text-decoration: underline;'>1</span>494</span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span> Hard Core (On The One)     Duane &amp; Co.                       <span style='text-decoration: underline;'>1</span>987   538   344</span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span> Jack My Body               Yellow House                      <span style='text-decoration: underline;'>1</span>987   402   359</span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span> Frequency (Out Of Control) Lil' Louis                        <span style='text-decoration: underline;'>1</span>987     4   460</span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span> Insane                     Suburban Boyz                     <span style='text-decoration: underline;'>1</span>988   340   608</span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 303 more rows</span></span></code></pre>

</div>

With this output, a visualisation can be knocked up to explore which Dance Mania
12-inches Discogs users want or own.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># load viz helpers</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://github.com/thomasp85/scico'>scico</a></span><span class='o'>)</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='http://github.com/ewenme/ewenthemes'>ewenthemes</a></span><span class='o'>)</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://github.com/slowkow/ggrepel'>ggrepel</a></span><span class='o'>)</span>

<span class='c'># plot</span>
<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span><span class='nv'>dm_community_metrics</span>, <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x<span class='o'>=</span><span class='nv'>have</span>, y<span class='o'>=</span><span class='nv'>want</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span><span class='o'>(</span><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>colour<span class='o'>=</span><span class='nv'>year</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/ggrepel/man/geom_text_repel.html'>geom_text_repel</a></span><span class='o'>(</span><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>label<span class='o'>=</span><span class='nf'><a href='https://rdrr.io/r/base/paste.html'>paste0</a></span><span class='o'>(</span><span class='nv'>artist</span>, <span class='s'>" - "</span>, <span class='nv'>title</span><span class='o'>)</span><span class='o'>)</span>, size<span class='o'>=</span><span class='m'>3</span>,
                  data <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/subset.html'>subset</a></span><span class='o'>(</span><span class='nv'>dm_community_metrics</span>, <span class='nv'>have</span> <span class='o'>&gt;=</span> <span class='m'>800</span> <span class='o'>|</span> <span class='nv'>want</span> <span class='o'>&gt;=</span> <span class='m'>1000</span><span class='o'>)</span>,
                  family <span class='o'>=</span> <span class='s'>"IBM Plex Sans"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>title<span class='o'>=</span><span class='s'>"Dance Mania 12\"s on Discogs"</span>, 
       subtitle<span class='o'>=</span><span class='s'>"How many Discogs user's own/want Dance Mania (1985 - 2018) 12-inches"</span>,
       x <span class='o'>=</span> <span class='s'>"Own it"</span>, y <span class='o'>=</span> <span class='s'>"Want it"</span>,
       caption<span class='o'>=</span><span class='s'>"source: Discogs | made by @ewen_"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://rdrr.io/pkg/ewenthemes/man/theme_ewen_rs.html'>theme_ewen_rs</a></span><span class='o'>(</span>grid <span class='o'>=</span> <span class='kc'>FALSE</span>, subtitle_size <span class='o'>=</span> <span class='m'>11</span>, subtitle_margin <span class='o'>=</span> <span class='m'>20</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>scico</span><span class='nf'>::</span><span class='nf'><a href='https://rdrr.io/pkg/scico/man/ggplot2-scales.html'>scale_colour_scico</a></span><span class='o'>(</span>palette <span class='o'>=</span> <span class='s'>"lajolla"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/guides.html'>guides</a></span><span class='o'>(</span>col <span class='o'>=</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/guide_colourbar.html'>guide_colourbar</a></span><span class='o'>(</span>direction <span class='o'>=</span> <span class='s'>"horizontal"</span>, title <span class='o'>=</span> <span class='s'>"Release year"</span>,
                               barheight <span class='o'>=</span> <span class='m'>0.5</span>, barwidth <span class='o'>=</span> <span class='m'>10</span>, title.vjust <span class='o'>=</span> <span class='m'>1</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span><span class='o'>(</span>legend.position <span class='o'>=</span> <span class='s'>"bottom"</span><span class='o'>)</span>
</code></pre>
<img src="figs/unnamed-chunk-4-1.png" width="640px" style="display: block; margin: auto;" />

</div>

## Close

For more on installation, development status and for (very welcome)
contributions or issues, check in at the
[Github repo](https://github.com/ewenme/discogger). I'll look to list features I
intend to introduce there - feel free to beat me to the punch on any of those.

> Music is a language, you see, a universal language. Sun Ra
