---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Send your data by email x

If the data to send is not too big, another interesting idea is to send it by email using the ‘gmailr’ package.

```r
#install.packages("gmailr")
#install.packages("tableHTML")
 
# Packages loading

library(gmailr)
# This one is usefull to transform a data frame into an HTML <table>

library(tableHTML)
 
# This will allow you to connect to gmail
# replace the fake value by your key and secret
# more info here: https://gargle.r-lib.org/articles/get-api-credentials.html

gm_auth_configure("mykey.apps.googleusercontent.com", "mysecret")
 
#transform the data frame 'df' to a html table
msg = tableHTML(df)
 
# Construct email
test_email <- gm_mime() %>%
              gm_to("another@example.com") %>%
              gm_from("me@example.com") %>%
              gm_subject("Email title") %>%
              gm_html_body(paste("Hi Mate,<br />
Here are the data you requested:<code>", msg,"<br /><br />Kind regards,<br />François"))
# end send it
gm_send_message(test_email)
```



