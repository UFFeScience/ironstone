# ironstone

<!---# SAMbA (Spark provenAnce MAnagement) -->
# Samba-RaP
>An extension of Apache Spark for scientific computational experiments

<p align="center">
  <img src="home.png" style="max-height: 160px"/>
</p>

## Overview

This repository presents a quick start guide for SAMbA, an extension over Apache Spark, which aims at running black-box native programs that handle raw data files. Furthermore, this engine collects, store, and query provenance and domain-specific data that were manipulated during the execution of scientific applications. With respect to the analytical capabilities, SAMbA provides runtime dataflow analysis based on the provenance traces.



## Quick Start Guide

SAMbA is an extension of [Apache Spark](https://spark.apache.org/), and as such, has all default characteristics of its baseline. In this way, most existing Spark codes, which depends only of Spark Core, are compatible with our implementation. So, in this quick start guide, we will show the only the methods that we implemented.

In the Spark, everything starts with the creation of [```SparkContext```](https://spark.apache.org/docs/2.2.1/api/java/org/apache/spark/SparkContext.html). This class is used to begin the transformation process and can receive as parameter the [```SparkConf```](https://spark.apache.org/docs/2.2.1/api/java/org/apache/spark/SparkConf.html). The ```SparkConf``` is used to configure an execution and in the Spark, and now this class has new methods:

- ```enableProvenance()``` and ```disableProvenance()``` : To enable e disable the provenance;
- ```enableVersionControl()``` and ```disableVersionControl()``` : To enable e disable the version control system;
- ```setScriptDir(directory: String)``` : Method used to inform the SAMbA where are the script or programs that will be executed by the runScientificApplication method;

By default, the provenance and the version control system are enabled.

### New RDD Operations


In the Spark, we handle the data through of creates [```RDD```](https://spark.apache.org/docs/2.2.1/api/java/org/apache/spark/rdd/RDD.html), to create one, we use the ```SparkContext``` that create one from some source of data. All data dealt with by one ```RDD``` is saved in the database of provenance. If you don't want it for a specific ```RDD```, just call the ```ignoreIt()``` method. It is useful if a transformation doesn't produce a relevant data.


#### Schema

As previously stated, all data handled by one RDD is saved in the database. To improve the information that will be saved there, you can use the Schema. Utilizing Schema, you can select only the relevant data or format it. To create a Schema, you should create a class and extends the interfaces class ```DataElementSchema[T]``` or ```SingleLineSchema[T]``` and implemented the methods. In both classes have the method ```getFieldsNames()``` that expect as a result an array of string with all attributes of data.

* Sample of code: ```SingleLineSchema``` - Use this kind of schema when the result has only one line.

```scala
import br.uff.spark.schema.SingleLineSchema

class SampleOfSingleLineSchemaOfString extends SingleLineSchema[String] {

  override def getFieldsNames(): Array[String] = Array("Att 1", "Att 2")

  //value = "Value 1;Value 2"
  override def splitData(line: String): Array[String] = {
    val data = line.split(";")
    Array(data(0), data(2))
  }
}
```
