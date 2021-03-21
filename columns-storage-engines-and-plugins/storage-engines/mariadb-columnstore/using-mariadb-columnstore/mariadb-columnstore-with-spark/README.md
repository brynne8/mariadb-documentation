# MariaDB ColumnStore with Spark

# Introduction

Apache Spark ([http://spark.apache.org/](http://spark.apache.org/)) is a popular open source data processing engine. It can be integrated with MariaDB ColumnStore utilizing the Spark SQL feature.

There are currently two possibilities to interact from Spark with ColumnStore. 
The first, is to use the ColumnStoreExporter which is part of the Bulk Data Adapters. ColumnStoreExporter can be used to export dataframes into existing tables in ColumnStore which is magnitudes faster than injecting Dataframes through JDBC. The second way is to use the MariaDB Java Connector and connect through JDBC. This is especially useful to read data from ColumnStore into Spark and to apply changes to ColumnStore's database structure through DDL.

# MariaDB ColumnStore Exporter

Connects Spark and ColumnStore through ColumStore's bulk write API.

## Configuration

The following steps outline installing and configuring the MariaDB ColumnStoreExporter to be available in the Spark runtime:

- The latest version of the MariaDB Bulk Data Adapters need to be installed. See additional [documentation](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk).
- The configuration file <em> /usr/local/spark/conf/sparks-default.conf </em> should be created or updated to point to the BulkWriteAPI and ColumnStoreExporter libraries. Their paths depend on the OS you are using.

For Debian 8, 9 and Ubuntu 16.04:

```sql
spark.driver.extraClassPath /usr/lib/javamcsapi.jar:/usr/lib/spark-scala-mcsapi-connector.jar
spark.executor.extraClassPath /usr/lib/javamcsapi.jar:/usr/lib/spark-scala-mcsapi-connector.jar
```

For CentOS 7:

```sql
spark.driver.extraClassPath /usr/lib64/javamcsapi.jar:/usr/lib64/spark-scala-mcsapi-connector.jar
spark.executor.extraClassPath /usr/lib64/javamcsapi.jar:/usr/lib64/spark-scala-mcsapi-connector.jar
```

#### Troubleshooting

- Depending on your Java environment you might have to manually link the C++ library libjavamcsapi.so to your java.library.path.

- Depending on your Python environment you might have to manually link the Python modules columnStoreExporter.py and pymcsapi.py, and the C++ library _pymcsapi.so to the Python packages directory used by Spark.

For Python 2.7 they can be found in:

```sql
/usr/lib/python2.7/dist-packages, for Debian 8, 9 and Ubuntu 16.04, and in
/usr/lib/python2.7/site-packages, for CentOS 7.
```

For Python 3 they can be found in:

```sql
/usr/lib/python3/dist-packages,  for Debian 8, 9 and Ubuntu 16.04, and in
/usr/lib/python3.4/site-packages for CentOS 7.
```

## Usage

ColumnStoreExporter is compatible with Python 2.7, Python 3 and Scala.

It has a fairly simple notation: ColumnStoreExporter.export(database, table, dataframe), but requires that dataframe and table have the same structure.

Here is a simple demonstration exporting a dataframe containing numbers from 0 to 127 and their ASCII representation using ColumnStoreExporter into an existing table created with following DDL:

```sql
CREATE TABLE test.spark (ascii_representation CHAR(1), number INT) ENGINE=COLUMNSTORE;
```

Python 2.7 / 3

```sql
# necessary imports
from pyspark import SparkContext
from pyspark.sql import SQLContext, Row
import columnStoreExporter

# get the spark session
sc = SparkContext("local", "MariaDB Spark ColumnStore Example")
sqlContext = SQLContext(sc)

# create the test dataframe
asciiDF = sqlContext.createDataFrame(sc.parallelize(range(0, 128)).map(lambda i: Row(number=i, ascii_representation=chr(i))))

# export the dataframe
columnStoreExporter.export("test","spark",asciiDF)
```

Scala

```sql
// necessary imports
import org.apache.spark.sql.{SparkSession,DataFrame}
import com.mariadb.columnstore.api.connector.ColumnStoreExporter

// get the spark session
val spark: SparkSession = SparkSession.builder.master("local").appName("MariaDB Spark ColumnStore Example").getOrCreate
import spark.implicits._
val sc = spark.sparkContext

// create the test dataframe
val asciiDF = sc.makeRDD(0 until 128).map(i => (i.toChar.toString, i)).toDF("ascii_representation", "number")

// export the dataframe
ColumnStoreExporter.export("test", "spark", asciiDF)
```

### Documentation

The following documents provide SDK documentation:

- Usage documentation for Spark ([PDF](/kb/en/mariadb-columnstore-with-spark/+attachment/spark_mcsapi_usage_1_2_3), [HTML](/kb/en/mariadb-columnstore-with-spark/+attachment/spark_mcsapi_usage_html_1_2_3)) and PySpark ([PDF](/kb/en/mariadb-columnstore-with-spark/+attachment/pyspark_mcsapi_usage_1_2_3), [HTML](/kb/en/mariadb-columnstore-with-spark/+attachment/pyspark_mcsapi_usage_html_1_2_3)) for 1.2.3 GA.

## Limitations

- ColumnStoreExporter currently can't handle Blob data types.
- The table needs to be existent and in the same structure of the dataframe to export.

# MariaDB Java Connector

Connects Spark and ColumnStore through JDBC.

## Configuration

The following steps outline installing and configuring the MariaDB Java Connector to be available to the spark runtime:

- The latest version of the MariaDB Java Connector should be downloaded from [https://mariadb.com/downloads/connector](https://mariadb.com/downloads/connector) and copied to the master node, e.g. under /usr/share/java.
- The configuration file <em> /usr/local/spark/conf/sparks-default.conf </em> should be created or updated to point to the jdbc directory:

```sql
spark.driver.extraClassPath /usr/share/java/mariadb-java-client-1.5.7.jar
spark.executor.extraClassPath /usr/share/java/mariadb-java-client-1.5.7.jar
```

## Usage

Currently Spark does not correctly recognize mariadb specific jdbc connect strings and so the <em> jdbc:mysql </em> syntax must be used. The followings shows a simple pyspark script to query the results from ColumnStore UM server columnstore_1 into a spark dataframe:

```sql
from pyspark import SparkContext
from pyspark.sql import DataFrameReader, SQLContext
url = 'jdbc:mysql://columnstore_1:3306/test'
properties = {'user': 'root', 'driver': 'org.mariadb.jdbc.Driver'}
sc = SparkContext("local", "ColumnStore Simple Query Demo")
sqlContext = SQLContext(sc)
df = DataFrameReader(sqlContext).jdbc(url='%s' % url, table='results', properties=properties)
df.show()
```

Spark SQL currently offers very limited push down capabilities, so to take advantage of ColumnStore's ability to perform efficient group by, then an inline table must be used, for example:

```sql
df = DataFrameReader(sqlContext).jdbc(url='%s' % url, 
    table='( select year, sum(closed_roll_assess_land_value) sum_land_value from property_tax group by year) pt',
                                      properties=properties)
```