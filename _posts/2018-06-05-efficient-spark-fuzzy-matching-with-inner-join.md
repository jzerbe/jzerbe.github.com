---
layout: post
title: efficient Apache Spark fuzzy matching with inner JOIN
tags:
- Apache Spark
- fuzzy matching
- string distance
- inner JOIN
published: true
---
I have a dataset in a Spark 1.6.3 environment, represented as a
[`JavaRDD<Row>`](https://spark.apache.org/docs/1.6.3/api/java/org/apache/spark/api/java/JavaRDD.html),
and I want to figure out the string distance from every row to every other row; an N^2 problem.
This is not going to end well ...

My distance calculation for each row is the
[Levenshtein distance](https://people.cs.pitt.edu/~kirk/cs1501/Pruhs/Spring2006/assignments/editdistance/Levenshtein%20Distance.htm)
of 3-5 columns of strings contained in each row averaged together, when compared against another row.
I am using the
[Apache Commons LevenshteinDistance](https://commons.apache.org/proper/commons-text/apidocs/org/apache/commons/text/similarity/LevenshteinDistance.html)
implementation.

Try after try, tweaking partitioning parameters or using streaming in clever ways, I continued to get
[`java.lang.OutOfMemoryError: GC overhead limit exceeded`](https://plumbr.io/outofmemoryerror/gc-overhead-limit-exceeded).

A few weeks of head-bashing frustration goes by and I stumble upon
<http://aseigneurin.github.io/2016/02/22/record-inkage-a-real-use-case-with-spark-ml.html>.
I get inspired to try the suggested _Find potential duplicates_ step BEFORE running a _Distance calculation_.
Guess what? It works!

The following is the meat of the important pre-filtering step done with an exact inner JOIN:

    DataFrame leftFeatureDF = featureDF;
    DataFrame rightFeatureDF = featureDF;
    for (String columnName : allInterestingColumnNames) {
      leftFeatureDF = leftFeatureDF.withColumnRenamed(columnName, columnName.concat("-left"));
      rightFeatureDF = rightFeatureDF.withColumnRenamed(columnName, columnName.concat("-right"));
    }
    JavaRDD<Row> rowJavaRDD = null;
    for (String columnName : allInterestingColumnNames) {
      final String leftColName = columnName.concat("-left");
      final String rightColName = columnName.concat("-right");

      final DataFrame joinedFeatureDF = leftFeatureDF.join(
          rightFeatureDF, leftFeatureDF.col(leftColName).equalTo(rightFeatureDF.col(rightColName)), "inner"
      );

      if (rowJavaRDD == null) {
        rowJavaRDD = joinedFeatureDF.toJavaRDD();
      } else {
        rowJavaRDD = rowJavaRDD.union(joinedFeatureDF.toJavaRDD()).distinct();
      }
    }

    return rowJavaRDD;

The `JavaRDD<Row>` you are left with only contains rows that have exact matches on at least one column of data.
At this point it is possible to score the `N^2/K` sized dataset (where `K` is very large),
by comparing the _left_ and _right_ columns in each row.
