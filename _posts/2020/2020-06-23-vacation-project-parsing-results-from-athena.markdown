---
layout: "post"
title: "Vacation Project: Parsing results from Athena"
date: "2020-06-23 17:57"
category: data
featured_image: images/2020/06/vacationlearning-parsingathenaqueryresults.png
---

Part of today was spent at Mount Rainier (Link: [Photos in iCloud](https://www.icloud.com/sharedalbum/#B0YJ8GySPJ06Nqx)), so I did not put in a full day's effort.  Today's updates (Link: [Commits in GitHub](https://github.com/kzbigboss/2020-06-vacation-learning/commits/master)) involve parsing the response from Athena's `get_query_results()` method.  It is not pretty but it does the job:

![vacationlearning-parsingathenaqueryresults](../../images/2020/06/vacationlearning-parsingathenaqueryresults.png)

## Basics behind parsing Athena
There is a pretty mixed bag out there as to the best way to work with query results from Athena.  Some people on Stack Overflow suggested parsing the metadata of the response to find and get the S3 object that contains the query results. Why?  Because the S3 object is already in CSV format making it super easy to work with.  I wanted to stick with working in the API, though, so I kept researching.

The approach I leaned on was to use a list comprehension to execute a command for each element in the heavily nested query response.  It actually works surprisingly well.  The challenge I could not figure out was how to save the results into a data structure other than a list/array.  I prefer to package the results into dictionary keyed by stock symbol... so I wrote a method to parse the parsed results after loading it into a data frame.  But hey, it works!

## Next steps
### Reconsider impacts of data types for down stream consumers
With today's updates, I am at the point where my app can become aware if a portion of the data stream needs repair.  Before I move forward, I want to reevaluate how I will tell down stream consumers of the missing minutes.  Right now, I am returning full datetime strings (eg: `2020-06-22 22:15:00.000`) but I know the Finnhub.io API is going to need unix epoch time (eg: `1592864100`).  I could avoid another transform method if I just kept everything in unix epoch timestamp.  All that said, I am going to reevaluate how I am pulling data out of Athena and serving it down stream.

### Wireframe a state machine
Before I build the next couple sets of SQS/Lambda to perform the repair operation, I think I am going to build the a wireframe of how I want my AWS Step Function to run.
