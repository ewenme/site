---
title: 'In search of the mixtape GOAT'
description: 'A data-driven tribute to the best of the mixtape era glow-ups.'
date: '2018-09-17'
slug: in-search-of-the-mixtape-goat
tags: ['music', 'd3']
keywords: ['datpiff', 'd3', 'lil wayne', 'mixtape', 'bubble chart']
image: 'https://github.com/rbind/ewenme/raw/master/static/img/2018-09-17-in-search-of-the-mixtape-goat/datpiff-bubble.png'
draft: no
---

![](xxx-art.jpg)

The modern definition of the hip-hop mixtape originates in hip-hop circles of
the 2000s, speaking to an artist's independently released projects that act as
precursors to "official" full-lengths.
[In previous generations](https://www.billboard.com/articles/columns/hip-hop/7669073/history-dj-drama-mixtape-evolution),
hip-hop mixtapes were more DJ-led and focused on the ingenuity of the edit, but
a DIY ethos has prevailed with a shift towards MCs as main attraction. The rise
of mainstream streaming services and fall of physical formats has challenged the
identity of the mixtape somewhat, but understanding how this artform has
reinvented itself around the current climate might help alleviate these
concerns.

Traditionally speaking, mixtapes are usually free and subsequently avoid growing
copyright issues associated with recording techniques like
[sampling](<https://en.wikipedia.org/wiki/Sampling_(music)>) (central to a
hip-hop beatmaker's wheelhouse). This helps partly explain why some mixtapes
have been accompanied by astonishing groundswells of support in spite of modest
investment, why mixtape culture is spoken about in reverential terms and why it
continues to be a medium for inspired work - the music is unashamedly
open-source in attitude, both in it's transmission to fans (i.e. for free) and
it's creation (i.e. freely plucking historic recordings).

You can actually see how this cultural phenomenon intersects with software
development. Take [Glitch's about page](https://glitch.com/about), specifically
the use of the word "remix", hinting that the act of a developer re-working an
app (that someone else built and shared) isn't that far removed from
[Frank Ocean flipping a Coldplay sample](https://www.youtube.com/watch?v=zD3lRLThVRY).
(I'm aware that, beyond staring at computer screens in the dark, the
developer-musician comparisons probably stop here).

![](nostalgia-ultra-art.jpg)

I wanted to toast this crossover in philosophy by sharing some data-driven
stories of the mixtape. This piece should be good ammo in heated mixtape legend
convos, at least...

## Mixtapes by the numbers

So, who was really putting
[Numbers On The Boards](https://www.youtube.com/watch?v=DawrlSwHUiM), when it
came to mixtapes? In truth, the ethereal nature of the beast and the multitude
of ways the product reaches ears without much care for tracking (these are free
and unattached to labels, remember) means that comprehensive mixtape statistics
are hard to come by.

A good place to start is [DatPiff](http://www.datpiff.com/), maybe the leading
dedicated website for hip-hop mixtape digging. In the
[celebrated section](http://www.datpiff.com/mixtapes/celebrated), you can
quickly spot download figures mapped to pre-eminent mixtape projects.

![](datpiff-screenshot.png)

Harvesting these figures and crunching the downloads for each artist can uncover
who _really_ runs the mixtape game.[^data] This comes with some caveats -
mixtape streams are ignored (not readily obtainable for all listings), as are
tapes not available on DatPiff (or those that have been taken down). Still, a
good idea of the state of play is well within reach (PS the next bit is best
enjoyed and explored on a desktop browser, if you have one handy)...[^bubbles]

{{< rawhtml >}}

<iframe width="100%" height="700" frameborder="0"
  src="https://observablehq.com/embed/@ewenme/mixtape-kings/2?cells=chart"></iframe>
{{< /rawhtml >}}

> As detailed in
> [Mike Bostock's bubble chart block](https://bl.ocks.org/mbostock/4063269),
> bubble charts aren't as perceptually accurate as bars. However, being less
> concerned with precise values means hundreds of values can be packed into the
> same chart area, which makes sense for this type of exploratory interactive.

## Weezy the 🐐

In case there was any doubt, Lil Wayne AKA Weezy AKA Tunechi comes out on top of
DatPiff mixtape downloads, clocking in at **~18 million**. This can be largely
attributed to his infamous mixtape run of 2004-2008, encompassing both the _Da
Drought_ and _Dedication_ series'. Of the former,
[#3](https://www.youtube.com/watch?v=VjGSyQU-YxQ) (2007) is widely regarded as
the best of the bunch and even fancied as the finest of it's class in critical
circles. Your fave could never etc etc.

![](weezy.gif)

Honorable mentions to a few of my favs (plus helpful pointers to some
best-in-class material):

- Gucci Mane (5.8 million): Put out a dozen mixtapes during a monster 2008-2010
  period. Check out
  [The Movie: Part 3 (Burrprint: 3D)](https://www.youtube.com/watch?v=9xpVLjvEPK4)
  (2009) first (BRRRRRR).[^brrr]
- Future (4.3 million): The master of twisted Bacchanalia rap, as prolific as
  Gucci. Start off with [56 Nights](https://www.youtube.com/watch?v=DXXZ3J_QIGw)
  (2015).
- Chance the Rapper (2.8 million):
  [Acid Rap](https://www.youtube.com/watch?v=e67Q-sTHVhc) (2013) and
  [Coloring Book](https://www.youtube.com/watch?v=H6C0dfq4ZWM) (2016) are dope
  and up-ended many expectations that independent artists and mixtapes were
  facing extinction.

I could go on, but why not head back to the interactive and explore some of the
music yourself 🕊

[^data]:
    All the code used to scrape DatPiff's download numbers, as well as the raw
    data itself, is stored in
    [this GitHub repo](https://github.com/ewenme/mixtapes). Check the `src`
    directory for code and `raw-data` for data in .csv format.

[^bubbles]:
    The interactive visualisation of artist mixtape downloads was made with
    [D3.js](https://d3js.org/) (inspired by
    [Mike Bostock's D3 Bubble Chart](https://beta.observablehq.com/@mbostock/d3-bubble-chart).
    Original code can be found in
    [this Observable notebook](https://beta.observablehq.com/@ewenme/mixtape-kings/2).
    It's embedded inside the R markdown document used to generate this post (via
    [blogdown](https://bookdown.org/yihui/blogdown/)), which you can access
    [here](https://github.com/rbind/ewenme/blob/master/content/blog/2018-09-17-in-search-of-the-mixtape-goat.Rmd)

[^brrr]:
    I can't leave without giving it up to Brooke for the
    [BRRR R package](https://github.com/brooke-watson/BRRR). No running of a
    slow function is complete without the East Atlanta Santa providing
    encouragement.
