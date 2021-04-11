---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Send and read  SEO data to Google Sheet x

```text
install.packages("googlesheets4")
library(googlesheets4)
gs4_deauth()


gs4_auth()
(ss <- gs4_create("fluffy-bunny", sheets = list(flowers = head(iris))))


class(ss)



gs4_deauth()
read_sheet("https://docs.google.com/spreadsheets/d/1U6Cf_qEOhiR9AZqTqS3mbMF3zt2db48ZP5v3rkrAEJY/edit#gid=780868077")

```

