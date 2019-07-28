---
layout: post
title: Study Notes - DataCamp's Intro to PySpark class
categories: studying
---

I have used DataCamp in the past to get ramped up on things like Git and R's Tidyverse.  I realized recently they have a couple PySpark classes.  Recently I have been using a lot of [AWS Glue](https://aws.amazon.com/glue/) which lets you leverage a serverless instance of Spark for ETL jobs.  I started a new habit of taking notes in markdown when I study something... so here are my notes while studying this course.

DataCamp's Intro to PySpark [(paid link)](https://www.datacamp.com/courses/introduction-to-pyspark).

---
## Study log

| Date       | Start Time | End Time |
|:-----------|-----------:|---------:|
| 2019-07-28 |    8:40 AM |  9:30 AM |
| 2019-07-22 |    1:45 PM |  2:35 PM |
| 2019-07-20 |    4:35 PM |  5:05 PM |
| 2019-07-18 |    4:55 PM |  5:05 PM |
| 2019-07-14 |    4:00 PM |  4:45 PM |

---
## Chapter 1: Getting to know Spark
* Spark is a cluster based compute solution.
* Ideal for computations that can benefit from parallel work
  * Work is pushed to cluster nodes, each node performing a sequence of work and returning the results back to a master node.
* To initiate work, you connect to a host that has the master node.  When you issue commands, the master node will instruct the worker nodes.
  * Connecting to a cluster requires you to instantiate the `SparkContext` class.
* The basic building block of Spark is an RDD: resilient distributed dataset.
  * Spark includes a construct as a "Spark DataFrame" that provides a way to interface with RDD objects.
  * You establish a `SparkSession` from a `SparkContext` in order to being interacting with data.  
    * Think of the context as the database and the session as the cursor.
    * Instantiate a `SparkSession` via the `SparkSession.builder.getOrCreate()` method.  Using this method assigns a new or already created SparkSession to a variable.
* If you prefer to manipulate data as a Pandas dataframe, you can convert a resultant Spark dataframe to a Pandas dataframe via the `.toPandas()` method.
* You can move data into the SparkSession via the `.createDataFrame()` method.
  * This can be used to copy data from a Pandas dataframe into a Spark dataframe.
  * Be warned this output is only available locally.  It's not added to the current SparkSession catalog so you can't use SparkSession methods like `.sql()` to interrogate the data.
  * You can create a temporary view to push data into the current SparkSession catalog.
    * Only the current SparkSession can access this object.
    * `.createTempView()` creates a SparkSession view.
    * `.createOrReplaceTempView()` creates or replaces a view.  This helps to avoid creating duplicate temp data objects.
* Overall Spark interactions:
![spark interactions]({{ site.baseurl }}/images/201907-spark_figure.png)
* You can read data directly into a SparkSession.  Using the `.read.{$TYPE}()` method on the SparkSession will return a Spark dataframe.


### Useful methods/functions
``` python
# Instantiate a Spark session
spark = SparkSession.builder.getOrCreate()

# List of tables available in current SparkSession
spark.catalog.listTables()

# Pass an SQL-like query to Spark, returns a dataset
query = "SELECT * FROM EMPLOYEES"
employees = spark.sql(query)
employees.show() # prints dataset to console

# Convert Spark dataframe to Pandas dataframe
pd_employees = employees.toPandas()

# Convert other dataframe to Spark dataframe
spark_employees = spark.createDataFrame(pd_employees)

# Create temp view in SparkSession
spark_employees.createOrReplaceTempView("new_employees")

# Read CSV data into SparkSession
file_path = "path/to/data/stadiums.csv"
stadiums = spark.read.csv(file_path, header=True) # uses first row as headers

```

## Chapter 2: Data Manipulation
* **Adding a column** can be accomplished via `.withColumn()` on a Spark dataset.
  * Resultant object must be of column type; can be accomplished by referencing another column in your expression.
  * Two arguments for `.withColumn` are name of new column and then column definition.
  * Spark will respond with a new dataset.
    * Spark datasets are immutable.
* **Filtering** is available via `.filter()`
  * You can pass arguments that look liked
    * Pandas, where you leverage column names
      * `teams.filter(teams.wins > 40)`
    * SQL, where you provide the equivalent of a where clause as a text string
      * `teams.filter("wins > 40")`
* Selection, similar to SQL's `SELECT` clause is the `.select()` method where each argument passed represents a column from the dataframe you're working on.
  * When passing arguments, either pass:
    * Names as string: `"field_name"`
    * Objects as type: `df.colName`
      * Passing as column objects lets you manipulate the column while you select it similar to using `.withColumn`
      * eg: `teams.select(team.wins/teams.losses).alias(win_loss_ratio)`
  * `.select()` vs `.withColumn`
    * `.select()` will return only the columns you specify.
    * `.withColumn` returns the **entire** dataframe along with the new columns you define.
    * Best practice is to select down to the columns you need to save overhead.
  * PySpark has a method named `.selectExpr()` that can take an SQL expression as a string
    * eg: `teams.selectExpr("wins/losses as win_loss_ratio")`
      * Above the "as" keyword is acting as the `.alias()` method.
* Aggregations in PySpark are grouped into `GroupedData` methods and called via `.groupBy()` followed by an aggregation function.
  - min example: `teams.filter(team.name == "Bears").groupBy().max("score").show()`
  - There's an entire class for grouped data frames named `pyspark.sql.GroupedData` to help replicate SQL group by statements.
    - You can pass column arguments into a `.groupBy()` method and Spark will behavior similar to SQL's `GROUP BY` clause.
  - You can also leverage the `.agg()` method to invoke any of the `pyspark.sql.functions` on grouped data.
    - This is helpful with calculations like standard deviations:
      - `by_month_dest.agg(F.stddev("dep_delay")).show()`
- Joins, the act of merging two data frames, is available via `.join()` and takes three arguments:
  1. the other data frame you want to reference in the join.
  2. name of the key column(s) as a string (aka `ON` in SQL).
  3. the type of join you want to perform (aka `INNER JOIN` or `LEFT OUTER JOIN` in SQL).
  - eg:
    ``` python
    flights_with_airports = flights.join(
      airports
      ,"dest"
      ,"leftouter"
      )
    ```
## Chapter 3: Machine Learning
- PySpark contains a machine learning module named `pyspark.ml`.
  - Two important methods:
    - `.transform()` of the `Transformer` class takes a dataframe and returns a dataframe with a new column appended.
    - `.fit()` of the `Estimator` class takes a dataframe and returns a model object.
- Data types need to be correctly stated for ML operations
  - Spark will attempt to infer data types, but that isn't a guarantee.
  - You can manually restate the data type of a dataframe's column by using `.cost()`.
    - `.cast()` is applied directly against a column object.  The only argument the method takes is the type of data type you are casting the original data type as.

### Useful methods/functions
``` python
** Note: spark variable below refers to an instance SparkSession **

# create a spark dataset from a known table in the catalog
teams = spark.table("teams")

# create a new column to express total games played from total of wins + losses
teams = teams.withColumn("total_games", teams.wins + teams.losses)
## Note: this will replace the previous instance of `team` with a new instance that
## includes the resultant column.  You can't update Spark datasets as they are
## immutable.

# select a subset of columns via string
selection = teams.select(
  "team_name"
  ,"team_city"
  ,"team_manager"
  )

# select subset of columns via df.colName Objects
selection = teams.select(
   teams.team_name
  ,teams.team_city
  ,team_manager
  )

# create an calculated column via column object arguments
## you can define the expression separate from the .select() method
win_loss_ratio = (teams.win/teams.losses).(alias(win_loss_ratio))
## now select both the columns and the expression
teams_expanded = teams.select(
   "team_name"
  ,"wins"
  ,"losses"
  ,win_loss_ratio
  )

# create an calculated column via SQL expression in Spark
teams_expanded = teams.selectExpr(
   "team_name"
  ,"wins"
  ,"losses"
  ,"wins / losses as win_loss_ratio"
  )

# print to screen the results of multiple filters and an aggregation
# eg: what's the largest score the Bears had for all recorded home games?
teams.\
  .filter(teams.team_name == "Bears") \
  .filter(teams.location_tag == "home") \
  .groupBy() \
  .max("score") \
  .show()

# rename a single column, helpful for avoiding ambiguity when joining dataframes
teams.withColumnRenamed("previousName", "CurrentName")

# join objects together
merged_df = first_df.join(
   second_df    # other df
  ,"key"        # join key
  ,"leftouter"  # join type
   )

# cast a column to a new data type
dataframe = dataframe.withColumn("col", dataframe.col.cast("new_type"))
```
