---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Compute ‘Internal Page Rank’

It is very much an adaptation of [Paul Shapiro](https://twitter.com/fighto) awesome [Script](https://gist.github.com/pshapiro/616b64a4e4399326c82c34734885d5bd) but Instead of using ScreamingFrog export file, we will use the data from a [Rcrawler](../crawl/rcrawler.md) crawl.

Lets crawl with the link data enabled

```r
Rcrawler(Website = "https://www.rforseo.com",  NetworkData = TRUE)
```

When it's done, The links will be stored in the `NetwEdges` variable.

```r
View(NetwEdges)
```

![](../.gitbook/assets/screenshot-2021-05-28-at-11.04.27-am.png)

  
  
We only want to first 2 column:

```r
links <- NetwEdges[,1:2] %>%
   #grabing the first two columns
   distinct() 

# loading igraph package
 library(igraph)

# Loading website internal links inside a graph object
 g <- graph.data.frame(links)
 
# this is the main function, don't ask how it works
 pr <- page.rank(g, algo = "prpack", vids = V(g), directed = TRUE, damping = 0.85)
 
# grabing result inside a dedicated data frame
 values <- data.frame(pr$vector)
 values$names <- rownames(values)
 
# delating row names
 row.names(values) <- NULL
 
# reordering column
 values <- values[c(2,1)]
# renaming columns
 names(values)[1] <- "url"
 names(values)[2] <- "pr"
 View(values)
```

![Internal Page Rank calculation](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-17-23.57.20.png)

Let make it more readable, we’re going to put the number on a ten basis, just like when the PageRank was a thing.

```r
#replacing id with url
values$url <- NetwIndex
# out of 10
 values$pr <- round(values$pr / max(values$pr) * 10)
#display
 View(values)
```

![](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-18-00.09.37.png)

On 15 webpages website, it’s not very impressive but I encourage you to try on a bigger website.

