---
layout: "post"
title: "Vacation Project: Day Three"
date: "2020-06-19 10:16"
category: vacation-project-2020-06
---

You see this image below?  It scares me.  This is how a query looks when you ask Athena to grab query results for you.  It mimics the rows and columns in terms of how a person would think of a query result.  The raw JSON file is available at the end of this post.  Let's take a step back and talk about how I got here.

![vacationlearning-athenaqueryresults](../../images/2020/06/vacationlearning-athenaqueryresults.png)

## Handling state in a serverless world
Both [AWS Step Functions](https://aws.amazon.com/step-functions/) and [AWS Glue Workflows](https://docs.aws.amazon.com/glue/latest/dg/workflows_overview.html) are attempting to help the same challenge: how do you handle state in a serverless world?  State, in simple terms, is the system's awareness of preceding activities.  Serverless really thrives when operations are stateless: wake up, take some input, pay no regard to what happened previously, and create some output.  In this vacation project, I have a few states we need to cycle through:
- Check: can we check that data is healthy?
- Repair: can we trigger the activities to repair the data?
- Validate: can we confirm the repair activities were successful?
- Initiate: can we signal to clients that everything is OK for processing?

## Query results as a starting point
I know less of AWS Step Functions than I do about AWS Glue.  Since this vacation project is learning exercise, I figure that is a good reason to start with Step Functions.  First, though, I need to get a handle on how the data in my data lake would help me figure out a starting point.  Hence the Athena query results that started this post.

The premise is that I need to execute a query to understand which stocks are missing timestamps for a given period of time.  I still need to put in some thought into what a good cadence is (every hour?  every 5 minutes? how do I ignore incoming data? etc..).  Regardless, I will need to parse query results to get started.  With choosing Step Functions as my first attempt, the simplest approach would be to parse the data directly in an AWS Lambda function.  The challenge is that AWS Lambda is a bare bones environment unlike AWS Glue that has a ton of large data handling packages at its disposal.  

[boto3, the AWS Python software development kit](https://aws.amazon.com/sdk-for-python/), has an Athena client.  I wrote a Lambda to grab query results just to see how the data looks.  One neat thing is that Athena saves both the metadata and the query results of prior queries.  That saved me from having to run a query each time I ran my own experimental Lambda.  I grabbed the "query execution ID" of a past query, fed it it into boto3's Athena client, and received a JSON response containing the query results.  That is when I saw the structure of the data in the response: it is a nested dictionary that mimics the rows and columns in terms of how we think of a table.

## I miss dataframes
Seeing the query results as a heavily nested object made me really appreciate and miss Python data frames.  Data frames would like me interact with the data as if it were a table rather than a nested data structure.  I could incorporate a [Lambda Layer](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) to help provide a package like [pandas](https://pandas.pydata.org) to bring the concept of data frames to this project.  Being honest, this is starting to get heavy and making me reconsider using Lambda.  Why?  Adding in Layers is another layer of complexity and does not necessary solve how to shape query results as a state machine's starting point.  To do this right means:
- Define a process to capture all results from a given query.
  - Includes handling continuation tokens and collapsing multiple fetches together.
- Build a method that parses through the results that aligns with how the query result was structured.
  - This means that I cannot easily reuse this method for other unrelated queries.
- Use a function to identify "gaps" or "problems" in the previous query result.
- Fire off the results to start individual repair operations.

## What's next?
More thinking and tinkering for sure.  I wonder if I should poke a little more AWS Glue before going deeper with Lambda + Step Functions.  I am not sure how much I want to work on this over the weekend, though.  I am on vacation, after all!

## Athena JSON file
Here is the text response from Athena's `GetQueryResults` action.  For more information, check out [the related Athena's API reference document](https://docs.aws.amazon.com/athena/latest/APIReference/API_GetQueryResults.html) to learn a little more about it.

```json
{
  "UpdateCount": 0,
  "ResultSet": {
    "Rows": [
      {
        "Data": [
          {
            "VarCharValue": "capture_datetime_hour"
          },
          {
            "VarCharValue": "AMZN_CNT"
          },
          {
            "VarCharValue": "GOOG_CNT"
          },
          {
            "VarCharValue": "WORK_CNT"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 16:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "55"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 15:00:00.000"
          },
          {
            "VarCharValue": "55"
          },
          {
            "VarCharValue": "53"
          },
          {
            "VarCharValue": "53"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 14:00:00.000"
          },
          {
            "VarCharValue": "58"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "52"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 13:00:00.000"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "55"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 12:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "52"
          },
          {
            "VarCharValue": "56"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 11:00:00.000"
          },
          {
            "VarCharValue": "51"
          },
          {
            "VarCharValue": "58"
          },
          {
            "VarCharValue": "53"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 10:00:00.000"
          },
          {
            "VarCharValue": "52"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "50"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 09:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "56"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 08:00:00.000"
          },
          {
            "VarCharValue": "55"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "53"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 07:00:00.000"
          },
          {
            "VarCharValue": "51"
          },
          {
            "VarCharValue": "58"
          },
          {
            "VarCharValue": "58"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 06:00:00.000"
          },
          {
            "VarCharValue": "53"
          },
          {
            "VarCharValue": "58"
          },
          {
            "VarCharValue": "58"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 05:00:00.000"
          },
          {
            "VarCharValue": "52"
          },
          {
            "VarCharValue": "58"
          },
          {
            "VarCharValue": "58"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 04:00:00.000"
          },
          {
            "VarCharValue": "51"
          },
          {
            "VarCharValue": "52"
          },
          {
            "VarCharValue": "53"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 03:00:00.000"
          },
          {
            "VarCharValue": "57"
          },
          {
            "VarCharValue": "55"
          },
          {
            "VarCharValue": "56"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 02:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "55"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 01:00:00.000"
          },
          {
            "VarCharValue": "55"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "52"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-18 00:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "57"
          },
          {
            "VarCharValue": "57"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-17 23:00:00.000"
          },
          {
            "VarCharValue": "57"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "57"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-17 22:00:00.000"
          },
          {
            "VarCharValue": "54"
          },
          {
            "VarCharValue": "56"
          },
          {
            "VarCharValue": "55"
          }
        ]
      },
      {
        "Data": [
          {
            "VarCharValue": "2020-06-17 21:00:00.000"
          },
          {
            "VarCharValue": "27"
          },
          {
            "VarCharValue": "24"
          },
          {
            "VarCharValue": "24"
          }
        ]
      }
    ],
    "ResultSetMetadata": {
      "ColumnInfo": [
        {
          "CatalogName": "hive",
          "SchemaName": "",
          "TableName": "",
          "Name": "capture_datetime_hour",
          "Label": "capture_datetime_hour",
          "Type": "timestamp",
          "Precision": 3,
          "Scale": 0,
          "Nullable": "UNKNOWN",
          "CaseSensitive": false
        },
        {
          "CatalogName": "hive",
          "SchemaName": "",
          "TableName": "",
          "Name": "AMZN_CNT",
          "Label": "AMZN_CNT",
          "Type": "bigint",
          "Precision": 19,
          "Scale": 0,
          "Nullable": "UNKNOWN",
          "CaseSensitive": false
        },
        {
          "CatalogName": "hive",
          "SchemaName": "",
          "TableName": "",
          "Name": "GOOG_CNT",
          "Label": "GOOG_CNT",
          "Type": "bigint",
          "Precision": 19,
          "Scale": 0,
          "Nullable": "UNKNOWN",
          "CaseSensitive": false
        },
        {
          "CatalogName": "hive",
          "SchemaName": "",
          "TableName": "",
          "Name": "WORK_CNT",
          "Label": "WORK_CNT",
          "Type": "bigint",
          "Precision": 19,
          "Scale": 0,
          "Nullable": "UNKNOWN",
          "CaseSensitive": false
        }
      ]
    }
  },
  "ResponseMetadata": {
    "RequestId": "ec14be0c-1c55-4642-94dd-481ca2b9f92e",
    "HTTPStatusCode": 200,
    "HTTPHeaders": {
      "content-type": "application/x-amz-json-1.1",
      "date": "Fri, 19 Jun 2020 04:40:58 GMT",
      "x-amzn-requestid": "ec14be0c-1c55-4642-94dd-481ca2b9f92e",
      "content-length": "5208",
      "connection": "keep-alive"
    },
    "RetryAttempts": 0
  }
}
```
