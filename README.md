# Amazon_Vine_Analysis
Is there a paid review bias in a selected dataset?

## Resources
Google Colab
pgAdmin version 6.15
AWS cloud database (this database was deleted upon completion of analysis)
Dataset: [Amazon Dataset](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Video_Games_v1_00.tsv.gz)
Spark version 3.2.3

## Overview of Analysis
This analysis was requested by the client (SellBy) to determine the impact of Amazon's Vine program on the outcome of reviews. The client specifically requested that.
this analysis be performed on items within the "Video Games" tag because of the impact that streamers and reviewing content creators are perceived to have on the sales
of items within this realm. The analysis was performed using Google Colab to **extract, transform, and load** once that was completed the data analyzed on a number of 
parameters specified by the client.

## Results
This section will be broken down in to three subsections **Process**, **Vine reviews**, and **non-Vine reviews** for clarity, the queries asked by the client are listed
below:

* How many reviews for each type are there?
* How many are 5 star?
* What percentage of those reviews do the two categories make up?

### Process
To start a **Colab** notebook was started and Spark and Java were activated, from there **pyspark.sql** was imported from **SparkSession** and drivers installed. After
that the data was read into **Colab** from a url using **SparkFiles**.

![This image shows the starting cell](https://github.com/dh4rt/Amazon_Vine_Analysis/blob/main/images/Spark_start.png)

After the data was imported a more refined table was needed so the **vine_df** was built using the code shown below, after the DataFrame was built a count was done to
determine who many reviews overall existed within the DataFrame the result was an enormous 1,785,997.

![This image shows vine_df](https://github.com/dh4rt/Amazon_Vine_Analysis/blob/main/images/vine_df.png)

The client however required more refined information and thus requested that only reviews with at least 20 total_votes be used going forward, so a filter was applied
to the **vine_df** which was titled **cleaned_vine_df** after this DataFrame was created a count was again done resulting in a total of 65,379 reviews.


The client again asked for a more refined DataFrame, particularly one which would have the helpful_votes make up at least 50% of the total_votes. This was done by
filtering the **cleaned_vine_df** and dividing helpful_votes by total_votes and filtering out reviews where the result was not at least 50%, this table would become 
the primary table around which the rest of the analysis was performed. This DataFrame was called **most_helpful_df**


### Vine Reviews
* How many reviews for each type are there?
To get this answer the **most_helpful_df** was filtered by the **vine** column to include the reviews that indicated Y, immediately after this a count was performed
which showed a result of 94 votes.

* How many are 5 star?
To determine how many of those review are five star, the **five_star_df** was filtered by reviews that indicated Y to Vine participation. The result was a total of 48
five star reviews being posted by participants in the Vine program

* What percentage of those reviews do the two categories make up?
To get this result number of five star Vine reviews called **paid_five_star_df.count()** was divided by the number of five star reviews called **five_star_df.count()**
and then multiplied by 100 to get the number to percentage level, the result is a jaw dropping **0.30%** of five star reviews in this category come from Vine users. 

### non-Vine Reviews

* How many reviews for each type are there?
There is a grand total of **40,471 non-Vine** reviews within the dataset, a number that that is enormous when compared to the number of Vine reviews.

* How many are 5 star?
In a mirrored process to what was done to achieve the Vine five star review **five_star_df** was filtered to show the number of those reviews that did not involve
Vine users and the result was that 15,663 five star reviews did not come from Vine users.

* What percentage of those reviews do the two categories make up?
To get this result, a mirrored process was performed using the **unpaid_five_star.count()** being divided by the **five_star_df.count()** and multiplied by 100 to get
a stunning **99.69%** of five star reviews not coming from Vine users.

## Summary
This analysis shows with fairly stunning results that Vine users do not appear to create any meaningful positivity bias. When 99.69% of the five star reviews in a
category are not coming from these "power users" you can consider your data fairly unskewed.

There is more analysis that could be done with this data set to see if/how Vine users might create purchasing bias:
* what sub-category of item do they review most highly? (controllers, games, trinkets, etc.)
* what the cost of the items with the highest reviews are?

