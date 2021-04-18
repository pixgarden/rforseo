---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Hunt down keyword cannibalization x

### What is keyword cannibalization?

if you put a lot of articles out there, at some point, some article will compete with one another for the same keywords in Google result pages. it’s what SEO people call ‘keyword cannibalization’.

### Does it matter SEO wise?

Sometimes it’s perfectly normal. I hope, for your sake, that several of your webpages show up when someone is typing your brand name in Google.

Sometimes it’s not. Let me give an example:

💭 Imagine you run an e-commerce website, with various page type: products, FAQ’s, blog posts, …

At some point, Google decided to make a switch:  a couple of Google search queries that were sending traffic to product pages, now display a blog post of yours.

Inside Google Analytics, SEO sessions count is the same. Your ‘Rank Tracking’ software will not bring you up position change.

And yet, these blog post pages will be able to convert much less, and at the end of the month, this will result in a decrease in sales.

### How to check for keyword cannibalization?

There are several ways to do it. Of course, SEO tools people want you to use their tools, the [method from ahref](https://ahrefs.com/blog/keyword-cannibalization/) is definitely useful. Unfortunately, this kind of tool can be sometimes imprecise, it doesn’t take into account what’s really happening.

So let me show you another method using R’. Once set up, you’ll be able to check big batches of keywords in minutes. 🤖



|  |
| :--- |


### step 3 – clean up

First, we’ll filter out queries that are not on the 2 first SERPs and that doesn’t generate any click. There is no point of making useless time-consuming calculations.

We’ll also remove branded search queries using a regex. As said earlier, having several positions for your brand name is pretty classic and shouldn’t be seen as a problem.

| 1234 | gsc\_queries\_filtered &lt;-gsc\_all\_queries %&gt;%                             filter\(position&lt;=20\) %&gt;%                             filter\(clicks!=0\) %&gt;%                             filter\(!str\_detect\(query, 'brandname\|brand name'\)\) |
| :--- | :--- |


_update this with your brand name_

### step 4 – computations

We want to know for one query, what percentage of clicks are going to each landing page.

First, we’ll create a new column **clicksT** with the aggregated number of clicks for each search query.  
Then, using this value to calculate what we need inside a new **per** column.

| 1234567 | gsc\_queries\_computed &lt;- gsc\_queries\_filtered %&gt;%                        group\_by\(query\) %&gt;%                        mutate\(clicksT= sum\(clicks\)\) %&gt;%                        group\_by\(page, add=TRUE\) %&gt;%                        mutate\(per=round\(100\*clicks/clicksT,2\)\) View\(gsc\_queries\_computed\) |
| :--- | :--- |


A **per** column value of 100 means that all clicks go the same URL.

Last final steps, we will sort rows

| 12 | gsc\_queries\_final &lt;- gsc\_queries\_computed %&gt;%                     arrange\(desc\(clicksT\)\) |
| :--- | :--- |


\[edit : \] It could also make sense fo remove rows where cannibalization is not significant. Where **per** column value is not very high. \[end of edit\]

Removing now useless columns: click, impression and total click per query group

| 1 | gsc\_queries\_final &lt;-gsc\_queries\_final\[,c\(-3,-4,-7\)\] |
| :--- | :--- |


Now it’s your choice to display it inside rstudio

| 1 | View\(gsc\_queries\_final\) |
| :--- | :--- |


Or write a CSV file to open it elsewhere

| 1 | write.csv\(gsc\_queries\_final,"./gsc\_queries\_final.csv"\) |
| :--- | :--- |


Here is my rstudio view \(anonymized sorry 🙊\)

![](https://www.gokam.fr/wp-content/uploads/2019/03/Screenshot-2019-03-19-19.23.34-copy.png)

### step 5 – analysis

You should check data inside each “query pack”. Everything is sorted using the total number of clicks, so, first rows are critical, bottoms rows not so much.

To help you deal with this, let’s check the first one’s

![](https://www.gokam.fr/wp-content/uploads/2019/03/seqrch-query-1.jpg)

_For Search query 1:_  
97% of click are going to the same page. Their is no Keyword cannibalization here. It’s interesting to notice that the ‘second’ landing page, only earn 1,4% of clicks, even though, it got an average position of 1,5. Users really don’t like the second ‘Langing page’. Page metadata probably sucks.

Check if the first landing page is the right one and we should move on.

![](https://www.gokam.fr/wp-content/uploads/2019/03/seqrch-query-2.jpg)

_For Search query 2:_  
63% of click are going to the first landing page. 36% to the second page. This is Keyword cannibalization.  
It could make sense to adapt internal linking between involved landing page to influence which one should rank before the other one’s, depending on your goals, pages bounce rates, etc.

And so on…

This is it my friends, I hope you’ll find it be useful!

