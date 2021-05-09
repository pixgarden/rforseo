# Load a batch of csv files

Aggregate several CSV files into one using file name as a column

```text
library(plyr)
library(readr)
library(purrr)

setwd("./Downloads/test/")



Tbl <- list.files(path = "./",
                  pattern="*.csv", 
                  full.names = T) %>% 
  map_df(function(x) read_csv(x, col_types = cols(.default = "c")) %>% mutate(filename=gsub(".csv","",basename(x)))) 


View(Tbl)
```



