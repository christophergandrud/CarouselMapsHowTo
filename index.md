---
title       : How-to Create Timeline Maps
subtitle    : with googleVis and Twitter Bootstrap's Carousel
author      : Christopher Gandrud
job         : Yonsei University
date        : November 2012
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
github:
  user: christophergandrud
  repo: CarouselMapsHowTo

--- 

## Motivation

<br />
<br />
<br />

### I like using [googleVis](http://code.google.com/p/google-motion-charts-with-r/) in R to quickly create nice interactive [Google Geomaps](https://developers.google.com/chart/interactive/docs/gallery/geomap).

---

## Example: Single Time Map

<iframe src = "http://dl.dropbox.com/u/12581470/Graphs/leg.violence.map.html"></iframe>

[Here are more details about this map.](http://dl.dropbox.com/u/12581470/Graphs/leg.violence.webpage.html)

---

## Motivation 

<br />
<br />
<br />

### This is great for individual maps.

<br />

### But I wanted to be able to put a series of Geomaps in a timeline and have them play as an animation or slide show.

--- 

## Current Tools

<br />

### There are a number of ways to create timeline maps in R. Here are some recent examples:

<br />

- An animated map of 2012 [US election campaign trips](http://civilstat.com/?p=876&utm_source=rss&utm_medium=rss&utm_campaign=animated-map-of-2012-us-election-campaigning-with-r-and-ffmpeg).

- Visualisation of [ocean shipping](http://sappingattention.blogspot.kr/2012/04/visualizing-ocean-shipping.html).

- Growth in [US Walmart stores](http://rstats.wordpress.com/2011/03/15/visualizing-store-growth/).

---

## Current Tools

<br />
<br />

- Many of these examples use the really helpful [animation](http://cran.r-project.org/web/packages/animation/index.html) package.

- The `animation` package pieces together a series of images created by R. These are playable as an HTML webpage, a GIF, a PDF, and so on. 

- However, I wasn't able to use the package with googleVis to produce interactive maps.

--- 

## The Solution

<br />

The solution I came up with was to use the [Carousel plugin](http://twitter.github.com/bootstrap/javascript.html#carousel) from [Twitter Bootstrap](http://twitter.github.com/bootstrap/).

<br />

- The Carousel plugin creates a slideshow with [jquery](http://jquery.com/). Usually Carousel slide shows include static images, but you can include `iframe`s in them as well.

- Placing the maps in `iframe`s in a Carousel `<div>` tag completely preserves their interactivity.

- The default formatting obviously matches nicely with Twitter Bootstrap produced [websites](http://builtwithbootstrap.com/).

---

## Example: Timeline Map

<iframe src="http://christophergandrud.github.com/amc-site/maps.html"></iframe>

Scroll down to see the whole map.

---

## The Steps: Overview

<br />

### There are 3 general steps to create interactive timeline maps with googleVis & Carousel:

<br />

1. Create a series of googleVis maps in R.

2. Include these maps in Carousel `div` tags on an HTML page.

3. Publish.

--- 

## The Steps: R.1

### First create a function to build the maps in R. 

I did this by writing a simple function I called `MapAMC` ([Full replication code](https://github.com/christophergandrud/amcData/blob/master/SourceCode/Descriptive/AMCTotalMaps.R)).

```r
library(googleVis)
MapAMC <- function(y){
                      yearTemp <- y  
                      TempMap <-  gvisGeoMap(subset(Full, year == yearTemp &
                                              TotalAmcCreated != 0), 
                                              locationvar = "ISOCode", 
                                              numvar = "TotalAmcCreated",
                                              options = list(
                                                colors = "[0xECE7F2, 0xA6BDDB, 0x2B8CBE]",
                                                width = "780px",
                                                height = "500px"
                                                ))
                                                
  print(TempMap, file = paste("AMCMap", yearTemp, ".html", sep = ""), tag = "chart")
}
```

---

## The Steps: R.2

### Then run the command.

I ran a vector of containing the years I wanted to map through the `MapAMC` function with `lapply`:

```r
Years <- c(1980, 1985, 1990, 1995, 2000, 2005, 2011)

lapply(Years, MapAMC)
```

In this example, seven HTML files will be saved in the working directory.

You probably want to have them put into the same directory that the framing website is in.

To make things tidy, I put the maps into a folder called `BaseMaps` in a subdirectory of the file where the website's markup file is located.

---

## The Steps: HTML.1

### Open the HTML file of the webpage you are using to frame the maps.

In the header, declare where `jquery` and the Carousel javascript files are located. 

I linked directly to Twitter Bootstrap's originals:

```html
<script type="text/javascript" 
  src="http://twitter.github.com/bootstrap/assets/js/jquery.js">
</script>

<script type="text/javascript" 
  src="https://raw.github.com/twitter/bootstrap/master/js/bootstrap-carousel.js">
</script>
```

**Note:** if you are using [Jekyll](http://jekyllbootstrap.com/) with [GitHub Pages](http://pages.github.com/) (recommended), you will want to include this code at the **end** of the markup file. 

---

## The Steps: HTML.2

### Now add the Carousel into your website's markup file.

A basic Carousel `div` tag structure goes something like this:

```html
<div id="MyMap" class="carousel slide">
  <div class="carousel-inner">
    <div class="active item">
      <iframe src="BaseMaps/AMCMap1980.html" height = 560 width = 800>
      </iframe>
        <div class="carousel-caption">
          1980
        </div>
      </div>
    </div
  </div>
</div>
```

[HERE](https://github.com/christophergandrud/amc-site/blob/gh-pages/maps.html) is the full code for the example.

---

## The Steps: Publish

<br />
<br />
<br />

### Finally publish the website however you want.

---

## Backmatter

### Contact:

If you have any suggestions or comments you can reach me through my [blog](http://christophergandrud.blogspot.com/) or [@ChrisGandrud](https://twitter.com/ChrisGandrud)

<br />

### Thanks to:

- [Markus Gesmann](http://lamages.blogspot.com/) for creating the googleVis package.

- The people behind Twitter Bootstrap, including [Mark Otto](https://twitter.com/mdo) and [fat-kun](https://twitter.com/fat).

- [Ramnath Vaidyanathan](https://twitter.com/ramnath_vaidya), who created the [Slidify](http://ramnathv.github.com/slidify/) package that I used to make this slide show in R.
