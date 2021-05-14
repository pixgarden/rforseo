# Send requests to the Google Indexing API using googleAuthR

_This guide has been written by_ [_Ruben Vezzoli_](https://twitter.com/RubenVezzoli) _in July 2020. Ruben is a 25 years old , Italian, Data Analyst checkout_[ _this website_ ](https://www.rubenvezzoli.online/)_with other interesting R scripts._

Two years ago, Google introduced the **Indexing API** with the intent of solving an issue that affected jobs/streaming websites – having outdated content in the index. The [Google Developers documentation](https://developers.google.com/search/apis/indexing-api/v3/using-api) says:

“_You can use the Indexing API to tell Google to update or remove pages from the Google index. The requests must specify the location of a web page. You can also get the status of notifications that you have sent to Google. Currently, the Indexing API can only be used to crawl pages with either job posting or livestream structured data._”

Many SEOs are using Indexing API also for non-job-related websites and that’s why I decided to build an R script to try out the API \(and it worked\).

I’m going to show you how the script works, but don’t forget that there is a **free quota of 200 URLs sent per day**!

### Create the Indexing API Credentials

First of all, you have to generate the client id and client secret keys for the APIs.

Open the [Google API Console](https://console.developers.google.com/apis/) and go to the API **Library**.

![](https://www.rubenvezzoli.online/wp-content/uploads/2020/07/indexing-api.jpg)

Open the Indexing API page and enable the API. Then go to the **Credentials** page and there you’ll find your credentials.

![](https://www.rubenvezzoli.online/wp-content/uploads/2020/07/indexing-api-credentials-1024x231.jpg)

### googleAuthR package & Indexing API options

I created the script using the [googleAuthR](https://code.markedmondson.me/googleAuthR) package, which allows you to send requests to Google APIs.

The code takes in **input,** a **character vector of maximum 200 URLs** and returns in a data frame the response of the API \(see the screenshot below\).

You can use the script to update or delete pages. You just have to change the **line 38**:

* Use type = “URL\_UPDATED” if you have to update the page
* Use type = “URL\_DELETED” if you have to remove the page

![](https://www.rubenvezzoli.online/wp-content/uploads/2020/07/output-indexing-api-1024x132.png)

```r
# LAST UPDATE: 20-03-2021
# Install & Load the packages
 
install.packages("googleAuthR")
install.packages("tidyverse")
install.packages("readr")
 
library("googleAuthR")
library("tidyverse")
library("readr")
 
# Set credentials and scope
 
clientId <- "PASTE HERE YOUR CLIENT ID"
clientSecret <- "PASTE HERE YOUR CLIENT SECRET" 
scope <- "https://www.googleapis.com/auth/indexing"
 
options("googleAuthR.client_id" = clientId, 
        "googleAuthR.client_secret" = clientSecret, 
        "googleAuthR.scopes.selected" = scope,
        "googleAuthR.verbose" = 0 # Not mandatory - I just use it to debug the script
        )
 
# Google API OAuth
 
gar_auth()
 
# List of URLs - Daily Limit of 200 URLs 
 
urls <- read_csv("~/Desktop/Your-file-name.csv")
urls <- urls[ ,1]
 
# indexingApi function - you can use the function to send requestes to the indexing API using urls vector as an input
# It also GET the response from the API and stores it in a data frame
 
indexingApi <- function(page) {
    
    body <-  list(
                  url = page,
                  type = "URL_UPDATED")
    
    f <- gar_api_generator("https://indexing.googleapis.com/v3/urlNotifications:publish",
                           "POST")
    
    result <- f(the_body = body)
    result <- as.data.frame(result[[6]][[1]][[2]])
    
    return(result)
    
}
 
# The API responses are stored in a data frame
 
APIResponse <- map_dfr(
                      .x = urls,
                      .f = indexingApi)
 
# You can download the API responses as .csv file
 
write.csv(APIResponse, "Your-file-name.csv")
```

