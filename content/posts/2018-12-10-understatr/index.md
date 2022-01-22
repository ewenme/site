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
output:
  hugodown::md_document:
    fig_asp: 0.8
rmd_hash: e84d1efca5984797
---

[understat](https://understat.com/) is a treasure trove of football statistics
and advanced metrics. All of your ~~"old man yells at cloud" conjuring~~
household favourites are here. Democratized access to xG for casuals everywhere,
so we can do unholy things like this.

<blockquote class="twitter-tweet" data-lang="en">
<p lang="und" dir="ltr">
<a href="https://t.co/cNiXEsfAms">pic.twitter.com/cNiXEsfAms</a>
</p>
--- Ryan O\'Hanlon (@rwohan) <a href="https://twitter.com/rwohan/status/1071159056096579585?ref_src=twsrc%5Etfw">December 7, 2018</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

While the site itself is easy to navigate and features nice off-the-shelf
exploratory viz tools (à la the above radar), many (myself included) can get a
bit fidgety without access to all this stuff in a structured format for
analysis.

So, I made a thing to grab understat data and tidy it up, so you can get on with
the interesting stuff. Say 👋 to
[understatr](https://github.com/ewenme/understatr), an
[R](https://www.r-project.org/) package that makes pulling xG numbers as easy as
🥧

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># load package</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://ewenme.github.io/understatr'>understatr</a></span><span class='o'>)</span>

<span class='c'># get dortmund playing squad data for this season</span>
<span class='nv'>dortmund</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://ewenme.github.io/understatr/reference/get_team_players_stats.html'>get_team_players_stats</a></span><span class='o'>(</span>team_name <span class='o'>=</span> <span class='s'>"Borussia Dortmund"</span>, year <span class='o'>=</span> <span class='m'>2018</span><span class='o'>)</span>

<span class='nv'>dortmund</span>
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 23 × 19</span></span>
<span class='c'>#&gt;    player_id player_name  games  time goals    xG assists    xA shots key_passes</span>
<span class='c'>#&gt;        <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;chr&gt;</span>        <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span>   <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span> <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span>      <span style='color: #555555; font-style: italic;'>&lt;dbl&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span>      <span style='text-decoration: underline;'>2</span>380 Paco Alcácer    26  <span style='text-decoration: underline;'>1</span>181    18 10.2        0  1.61    52         15</span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span>       319 Marco Reus      27  <span style='text-decoration: underline;'>2</span>335    17 16.6        8  6.94    69         38</span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span>      <span style='text-decoration: underline;'>6</span>345 Jadon Sancho    34  <span style='text-decoration: underline;'>2</span>463    12  7.42      14  8.31    40         63</span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span>       422 Mario Götze     26  <span style='text-decoration: underline;'>1</span>694     7  4.84       7  6.64    28         41</span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span>      <span style='text-decoration: underline;'>2</span>662 Christian P…    20   916     4  5.16       4  2.97    27         21</span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span>      <span style='text-decoration: underline;'>3</span>059 Axel Witsel     33  <span style='text-decoration: underline;'>2</span>873     4  2.88       1  2.03    36         16</span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span>      <span style='text-decoration: underline;'>5</span>230 Thomas Dela…    30  <span style='text-decoration: underline;'>2</span>302     3  2.76       5  2.90    33         15</span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span>      <span style='text-decoration: underline;'>3</span>455 Raphael Gue…    23  <span style='text-decoration: underline;'>1</span>539     2  2.88       6  3.94    35         38</span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span>      <span style='text-decoration: underline;'>5</span>197 Achraf Haki…    21  <span style='text-decoration: underline;'>1</span>740     2  1.69       4  2.96    20         11</span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span>      <span style='text-decoration: underline;'>5</span>355 Jacob Bruun…    24  <span style='text-decoration: underline;'>1</span>267     2  3.27       2  3.16    26         17</span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 13 more rows, and 9 more variables: yellow_cards &lt;dbl&gt;,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   red_cards &lt;dbl&gt;, position &lt;chr&gt;, team_name &lt;chr&gt;, npg &lt;dbl&gt;, npxG &lt;dbl&gt;,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   xGChain &lt;dbl&gt;, xGBuildup &lt;dbl&gt;, year &lt;dbl&gt;</span></span></code></pre>

</div>

You can use this in conjunction with the [tidyverse](https://www.tidyverse.org/)
to get from data to viz lighting-quick.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://tidyverse.tidyverse.org'>tidyverse</a></span><span class='o'>)</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='http://github.com/ewenme/ewenthemes'>ewenthemes</a></span><span class='o'>)</span> <span class='c'># (a personal chart themes pkg)</span>

<span class='nv'>dortmund</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span> 
  <span class='c'># remove players w/ zero xG</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span><span class='o'>(</span><span class='nv'>xG</span> <span class='o'>&gt;</span> <span class='m'>0</span><span class='o'>)</span> <span class='o'><a href='https://magrittr.tidyverse.org/reference/pipe.html'>%&gt;%</a></span> 
  <span class='c'># chart xG in desc order</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span><span class='o'>(</span><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span><span class='o'>(</span>x <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/stats/reorder.factor.html'>reorder</a></span><span class='o'>(</span><span class='nv'>player_name</span>, <span class='nv'>xG</span><span class='o'>)</span>, y <span class='o'>=</span> <span class='nv'>xG</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># make it a bar chart</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_col</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># flip the bars</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/coord_flip.html'>coord_flip</a></span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># add some labels (making sure to credit understat!)</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span><span class='o'>(</span>title <span class='o'>=</span> <span class='s'>"Expected goals contributions"</span>, subtitle <span class='o'>=</span> <span class='s'>"Borussia Dortmund, 2018/19"</span>,
       x <span class='o'>=</span> <span class='kc'>NULL</span>, caption <span class='o'>=</span> <span class='s'>"source: understat.com | @ewen_"</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='c'># add my personal chart theme</span>
  <span class='nf'><a href='https://rdrr.io/pkg/ewenthemes/man/theme_ewen_rs.html'>theme_ewen_rs</a></span><span class='o'>(</span>grid <span class='o'>=</span> <span class='s'>"X"</span>, axis <span class='o'>=</span> <span class='kc'>FALSE</span>, axis_text_size <span class='o'>=</span> <span class='m'>9</span><span class='o'>)</span>
</code></pre>
<img src="figs/dortmund-plot-1.png" width="100%" style="display: block; margin: auto;" />

</div>

Peep the project's GitHub page for updates, for now at least. As noted in the
[readme](https://github.com/ewenme/understatr/blob/master/README.md), while
non-commercial use of the data is fine for now, I don't own it and similarly
don't control changes to the website. I say this to say that `understatr` may
break (or become illegal) in future, so enjoy it while you can 🎈 I'd be psyched
to hear about people using it, or even contributing to it. Esp. here for the
hottest of hot take Messi radar trolls, please cite the proj in all of those.
