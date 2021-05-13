---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Send and read SEO data to Excel/CSV

CSV and Excel file remain one of amongst the most well-used file formats for exchange data.

### Read your data from a CSV

```text
#setup where to read the file
setwd("~/Desktop")
# en write the file
test <- read.csv(df, "data.csv")
```

### Export your data into a CSV

assuming your data is store inside **df** var, fairly simple:

```text
#setup where to write the file
setwd("~/Desktop")
# en write the file
write.csv(df, "data.csv")
```

### Read an excel

```text
# the file.choose() will prompte a file selector
# the 1 say we want to load the first tab
test <- xlsx::read.xlsx(file.choose(),1)
```

### Export your data into an excel file

A little bit more complex, we’ll use the ‘xlsx’ package

```text
#setup where to write the file
setwd("~/Desktop")
 
# if the package is not instal yet, run this  
# install.packages("xlsx")
 
# Loading the package 
library(xlsx)
 
# we write the file 
write.xlsx(df, "data.xlsx")

```

A few more tips for you:

I’ll like to use the **sheetName** option to explicitly name the tab. The default name is “Sheet1”. Quite useful to have a record of when the file has been generated for example. Replace last instruction what follows and you’ll be able to know.

```text
write.xlsx(df, "data.xlsx", sheetName=format(Sys.Date(), "%d %b %Y"))
```

Another good one that I like is to send the excel file to a Shared folder directly. Replace first instruction by

```text
setwd("/Users/me/Dropbox/Public")
```

Of course, replace the file path with yours.

### Import and merge a batch of CSV files

Aggregate several CSV files into one using file name as a column

```text
library(plyr)
library(readr)
library(purrr)

# add the path where the csv's are located
setwd("./Downloads/test/")

# list csv files inside the directory
# for each: import the csv (read.csv function)
# add filename as a column
# and merge

Tbl <- list.files(path = "./",
                  pattern="*.csv", 
                  full.names = T) %>% 
                  map_df(function(x) read_csv(x, col_types = cols(.default = "c")) %>%
                  mutate(filename=gsub(".csv","",basename(x)))) 


View(Tbl)
```

