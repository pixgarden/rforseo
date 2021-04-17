---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Forecast 🚀 x

```text
path <- file.choose()


df <- read.csv(path, skip = 6)

View(df)

df <- data<-na.omit(df)

library(lubridate)
library(prophet)

origin_date <- ymd("2019-01-01")

origin_date + ddays(1)

df$index <- as.numeric(rownames(df))-1

df$ds <- origin_date+ddays(df$index-1)

df$ds <- df$Day.Index

df$y <- df$Sessions

df$Sessions <- NULL
df$Day.Index <- NULL


#ggplot(df)

m <- prophet(df)


future <- make_future_dataframe(m, periods = 365)

# R
forecast <- predict(m, future)
tail(forecast[c('ds', 'yhat', 'yhat_lower', 'yhat_upper')])


# View(forecast)

plot(m, forecast)

prophet_plot_components(m, forecast)

dyplot.prophet(m, forecast)


```

