# Explore data with rPivotTable ✔️

[rpivotTable](https://cran.r-project.org/web/packages/rpivotTable/vignettes/rpivotTableIntroduction.html)  is a great paclage by Enzo Martoglio that allow you explore a small dataset using am HTML drag&drop interface that looks like the pivot interface from Google Sheet or Excel.

### Install and load rPivotTable <a id="4-explore-crawled-data-with-rpivottable"></a>

as usual the instruction are quite

```r
#install package rpivottable ONLY the first time
install.packages("rpivottable")
# And loading 
library(rpivottable)
```

Imagine you want explorer a data frame called MERGED just sent

```r
# launch 
toolrpivotTable(MERGED)
```

it will open this HTML and drag and dropping KPIs from the left column

![This create a drag &amp; drop pivot explorer](https://www.gokam.co.uk/wp-content/uploads/2020/08/LgfVsFu6NL.gif)

You can also use the top dropdown list to make it display a plot instead of a table.

![It&#x2019;s also possible make some quick data viz](https://www.gokam.co.uk/wp-content/uploads/2020/08/UmtYC25Kdh.gif)



[Here is a demo HTNL file ](https://www.gokam.co.uk/rpivottable.html)to try it yourself.

