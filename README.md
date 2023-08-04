# IRONSTONE (Data-Oriented Approach for Routing Vehicles To Respond Emergency Events)

>IRONSTONE is a data-oriented approach that aims at defining vehicle routes for responding to emergency events (e-events) by consuming real-time traffic data. 

<p align="center">
  <img src="Graphical+Abstract.png" style="max-height: 160px"/>
</p>

## Overview

This repository presents a quick start guide for IRONSTONE. IRONSTONE is an innovative approach that analyzes real-time traffic data and generates a graph representing the streets within a radius (e.g., 10 km) from the emergency event (i.e., e-event). Based on this graph, IRONSTONE employs a multi-start random constructive heuristic to schedule vehicles efficiently for responding to e-events. In its current version, IRONSTONE utilizes the Tomtom API to estimate travel times between the vehicle locations and the e-events. Additionally, historical data is employed to estimate the time required for responding to each e-event. Once a specific e-event is addressed, the proposed approach regenerates the routes, considering the new registered events and traffic conditions. IRONSTONE is designed with the primary objective of minimizing the total response time. The objective function is structured to minimize the sum of the times required to respond to each e-event. This ensures a swift and effective response to emergency situations.

## Quick Start Guide

For ensuring reproducibility, IRONSTONE has been implemented as a Jupyter Notebook. To execute IRONSTONE successfully, the user must have a computer with Jupyter Notebook version Anaconda3 installed. Additionally, a Tomtom API Key is required, which can be obtained for free in [Tom Tom](www.tomtom.com)  website.

The following steps outline the execution process:

1. Obtain the Tomtom API Key from [Tom Tom](www.tomtom.com) for free.
2. Open the Jupyter Notebook on your computer.
3. Locate and open the "Generate Graph" Notebook.
4. Run the cells in the "Generate Graph" file in the exact order they appear.
5. After the "Generate Graph" process is completed, locate and open the "Heuristic" Notebook.
6. Run the cells in the "Heuristic" file in the exact order they appear.

By following these steps and running the cells as indicated, IRONSTONE will be executed effectively, allowing you to use its capabilities for analyzing real-time traffic data and generating optimal vehicle response routes for emergency events.

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
