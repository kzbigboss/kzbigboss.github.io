---
layout: "post"
title: "Road trip delay, so let's build a data pipeline!"
date: "2021-08-18 15:16"
featured_image: images/2021/08/teslafipipeline.jpg
category: data
---

Today was supposed to mark the beginning of my road trip.  Oops! I am grateful for the delay as I got to see some great people and build some data goodness instead.

## Life
Yesterday was supposed to be a day spent packing and preparing the car for the road; instead, I visited with friends all throughout the day.  Totally worth it.

## The itch to build
It was annoying me that I was drawing maps by downloading CSV files from [TeslaFi](www.teslafi.com).  I figured since I had an extra morning, I would automate some of the Tesla data collection.  This would enable me to query data directly in my drawing tools.  I envisioned the following solution:

![](/images/2021/08/teslafipipeline.jpg)

This solution's main activities include:
- Grab TeslaFi's access token via [Amazon AWS Secrets Manager](https://aws.amazon.com/secrets-manager/).
- Use a Python [AWS Lambda](https://aws.amazon.com/lambda/) to ping TeslaFi's API and collect a payload representing Tesla vehicle data.
- Push said payload into an [Amazon Kinesis Data Stream](https://aws.amazon.com/kinesis/data-streams/) and then store it in [Amazon S3](https://aws.amazon.com/s3/) via [Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/).
- Describe the data in an [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/populate-data-catalog.html) so that tools like [Amazon Athena](https://aws.amazon.com/athena/) can query it.

All in all, it took me about two hours to piece together. I leaned heavily on code snippets from my previous [2020 Vacation Project](https://github.com/kzbigboss/2020-06-vacation-learning) effort.  Honestly?  The longest part was setting up a virtual Python interpreter on my laptop... for whatever reason `virtualenv` was being weird and I did not want to muck up my laptop's system interpreter.

As for how it is executed, everything is encapsulated as a state machine in [AWS Step Functions](https://aws.amazon.com/step-functions).  This enables me to ping the API every 20 seconds.

## The results
I figured I would test the new pipeline out over lunch... so I enabled a state machine trigger and set out to visit [Sunfish on Alki Beach](https://goo.gl/maps/YAM1wcgDv99qPDC47).  When I returned, I used Tableau to connect via Athena to draw this:

![](/images/2021/08/gpsroutealkilunch.png)

Not bad for a quick morning project!  

The resultant source code is available in a new repo: [2021-epic-road-trip](https://github.com/kzbigboss/2021-epic-road-trip).  I will be using this repository to build more things as I play with data during this road trip.
