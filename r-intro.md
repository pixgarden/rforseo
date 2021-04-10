---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# it's an R World X

## The power of R. What's different about it?



## 'There is a package for that'



## The confusing things about R

### The name

"R" is not the most Google-friendly name. So here a few sources to help:

* [https://rseek.org/](https://rseek.org/) r search engine
* [https://www.r-bloggers.com/](https://www.r-bloggers.com/) r blogs aggregator
* [https://www.bigbookofr.com/](https://www.bigbookofr.com/) all the R free books
* [https://github.com/search?l=R&q=seo&type=code](https://github.com/search?l=R&q=seo&type=code) github r source code search

### &lt;- 

If you've seen some R' code before and you might have been surprised to see this &lt;-  being used . it's just a legacy thing, historically R differentiate  "assignation"  and "comparison", example:

If you want to set the value of X to 3.   

```r
x <- 3
```

if you want to make a comparison

```r
if (x = 3) {
    return "the value of X is 3"
}
```

If you want to keep this little _tradition_ alive you can do the same thing or use **=**

```r
x = 3
x <- 3
```

those are actually [nearly](https://stackoverflow.com/questions/1741820/what-are-the-differences-between-and-assignment-operators-in-r) equivalent.

#### %&gt;%

 **%&gt;%** operator, introduced by [magrittr](https://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html) package, is like the **&gt;** \( “pipe”\) for terminal command line. The operations are carried out successively. Meaning the results of the previous command are the entries for the next one.

Always better with an example:

```r
# those are equivalent
mean(abs(1))
#
1 %>% abs() %>% mean()
```

It seems a bit silly like that its   
more info on this [https://r4ds.had.co.nz/pipes.html](https://r4ds.had.co.nz/pipes.html)

