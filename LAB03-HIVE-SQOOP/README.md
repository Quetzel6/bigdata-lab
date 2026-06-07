# LAB03 — MapReduce, Hive and Hive with JSON

## Objective

This lab introduces core Hadoop data processing technologies including:

* Hadoop MapReduce
* Apache Pig
* Apache Hive
* Hive JSON SerDe
* Parquet Storage Format
* YARN Resource Management

Students will learn how to process data using multiple Hadoop ecosystem 
tools and compare different storage formats.

---

# Technologies Used

* AWS EMR
* Hadoop HDFS
* YARN
* MapReduce
* Apache Pig
* Apache Hive
* JSON SerDe
* Parquet

---

# Part 1 — Hadoop MapReduce WordCount

MapReduce is Hadoop's distributed processing framework.

In this exercise we use the classic WordCount example to count the number 
of occurrences of each word in a text file stored in HDFS.

---

## Step 1.1 Create WordCount Source File

Create a Java source file:

```bash
nano WordCount.java
```

Copy the WordCount v1.0 source code from:

https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html

Save the file.

---

## Step 1.2 Create Build Directory

```bash
mkdir wordcount_classes
```

Verify:

```bash
ls
```

---

## Step 1.3 Compile WordCount

```bash
javac \
-classpath 
/usr/lib/hadoop/client/hadoop-common.jar:/usr/lib/hadoop/client/hadoop-mapreduce-client-core.jar 
\
-d wordcount_classes/ \
WordCount.java
```

Verify:

```bash
find wordcount_classes
```

---

## Step 1.4 Package as JAR

```bash
jar -cvf wordcount.jar -C wordcount_classes/ .
```

Verify:

```bash
ls -lh wordcount.jar
```

---

## Step 1.5 Execute MapReduce Job

```bash
yarn jar wordcount.jar WordCount input/* output/wordcount
```

Expected output:

```text
Submitted application ...
```

---

## Step 1.6 View Result

List generated files:

```bash
hadoop fs -ls output/wordcount
```

Expected:

```text
part-r-00000
_SUCCESS
```

Display result:

```bash
hadoop fs -cat output/wordcount/part-r-00000
```

Example:

```text
Hello 1
World 1
Hadoop 2
```

---

# Part 2 — Monitor YARN

YARN (Yet Another Resource Negotiator) manages cluster resources and job 
execution.

---

## Step 2.1 Open Resource Manager

Open browser:

```text
http://[PRIMARY-NODE]:8088
```

Verify:

* Running Applications
* Completed Applications
* Cluster Metrics
* Available Resources

---

## Step 2.2 Verify WordCount Job

Locate the WordCount application and verify:

* Application ID
* Start Time
* Finish Time
* Status = FINISHED

---

# Part 3 — Apache Pig WordCount

Apache Pig provides a higher-level scripting language for Hadoop 
processing.

---

## Step 3.1 Create Pig Script

Create file:

```bash
nano wordcount.pig
```

Insert:

```pig
A = load 'input/*';
B = foreach A generate flatten(TOKENIZE((chararray)$0)) as word;
C = group B by word;
D = foreach C generate COUNT(B), group;
store D into 'output/wordcount-pig';
```

Save file.

---

## Step 3.2 Execute Pig Script

```bash
pig wordcount.pig
```

Expected:

```text
Success!
```

---

## Step 3.3 Verify Pig Output

```bash
hadoop fs -ls output/wordcount-pig
```

Display result:

```bash
hadoop fs -cat output/wordcount-pig/part-v001-o000-r-00000
```

Compare the output with MapReduce WordCount results.

---

# Part 4 — Apache Hive

Hive provides a SQL-like interface for querying large datasets stored in 
Hadoop.

---

## Step 4.1 Create Sample Data

Create file:

```bash
nano member1.txt
```

Insert:

```text
1,John,Smith,2006-02-15 04:34:33
2,Sawasdee,Thailand,2006-01-01 01:01:01
```

Save file.

---

## Step 4.2 Start Hive

```bash
hive
```

---

## Step 4.3 Create Table

```sql
CREATE TABLE member (
    mem_id INT,
    first_name STRING,
    last_name STRING,
    last_update TIMESTAMP
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/hadoop/member';
```

---

## Step 4.4 Show Table Information

```sql
SHOW TABLES;

DESC member;
```

---

## Step 4.5 Import Data

```sql
LOAD DATA LOCAL INPATH '/home/hadoop/member1.txt'
INTO TABLE member;
```

Expected:

```text
Loading data to table default.member
```

---

## Step 4.6 Query Data

```sql
SELECT * FROM member;
```

Expected:

```text
1 John Smith
2 Sawasdee Thailand
```

---

## Step 4.7 Count Records

```sql
SELECT COUNT(*) FROM member;
```

Expected:

```text
2
```

---

# Part 5 — Add Additional Data to Hive

## Step 5.1 Create Additional File

Create file:

```bash
nano member2.csv
```

Insert:

```text
3,xxx,yyy,2006-02-15 04:34:33
```

Save file.

---

## Step 5.2 Upload File to HDFS

```bash
hadoop fs -put member2.csv member
```

Verify:

```bash
hadoop fs -ls member
```

---

## Step 5.3 Query Updated Data

Start Hive again:

```bash
hive
```

Execute:

```sql
SELECT * FROM member;
```

Expected:

```text
1 John Smith
2 Sawasdee Thailand
3 xxx yyy
```

This demonstrates that Hive automatically reads files added to the table 
location.

---

# Part 6 — Hive with JSON

Hive can process JSON files using JSON SerDe.

---

## Step 6.1 Install JSON SerDe Library

Run as root:

```bash
sudo su -

wget -nv \
http://www.congiu.net/hive-json-serde/1.3.8/cdh5/json-serde-1.3.8-jar-with-dependencies.jar 
\
-O /usr/lib/hive/auxlib/json-serde-1.3.8-jar-with-dependencies.jar

exit
```

---

## Step 6.2 Start Hive

```bash
hive
```

---

## Step 6.3 Create JSON Table

```sql
CREATE EXTERNAL TABLE minbars (
    Symbol STRING,
    `Timestamp` STRING,
    Day INT,
    Open DOUBLE,
    High DOUBLE,
    Low DOUBLE,
    Close DOUBLE,
    Volume INT
)
ROW FORMAT SERDE
'org.openx.data.jsonserde.JsonSerDe'
LOCATION '/user/hadoop/minbars';
```

Exit Hive:

```sql
exit;
```

---

## Step 6.4 Download JSON Dataset

```bash
wget -O minbars.json \
https://www.dropbox.com/scl/fi/h0tpb09u3uuhlqzl9kh1q/minbars.json?rlkey=fkt7pvg97urmbwgtz1izssytn&dl=0
```

Verify:

```bash
cat minbars.json
```

---

## Step 6.5 Upload JSON to HDFS

```bash
hadoop fs -put minbars.json minbars
```

Verify:

```bash
hadoop fs -ls minbars
```

---

## Step 6.6 Query JSON Data

```bash
hive
```

Execute:

```sql
SELECT * FROM minbars;
```

Expected output should display stock market records stored in JSON format.

---

# Part 7 — GZIP Support

Hive can read compressed GZIP files directly.

---

## Step 7.1 Compress JSON File

```bash
gzip minbars.json
```

Verify:

```bash
ls
```

Expected:

```text
minbars.json.gz
```

---

## Step 7.2 Upload GZIP File

```bash
hadoop fs -put minbars.json.gz minbars
```

---

## Step 7.3 Remove Original JSON

```bash
hadoop fs -rm minbars/minbars.json
```

---

## Step 7.4 Query Again

```bash
hive
```

Execute:

```sql
SELECT * FROM minbars;
```

Expected result:

The query should still return records successfully.

This demonstrates Hive's transparent support for GZIP compressed files.

---

# Part 8 — Parquet Format

Parquet is a columnar storage format optimized for analytics workloads.

---

## Step 8.1 Create Parquet Table

```sql
CREATE EXTERNAL TABLE minbars2 (
    Symbol STRING,
    `Timestamp` STRING,
    Day INT,
    Open DOUBLE,
    High DOUBLE,
    Low DOUBLE,
    Close DOUBLE,
    Volume INT
)
STORED AS PARQUET;
```

Note:

Hive will create the table in the default warehouse location.

---

## Step 8.2 Insert Data into Parquet Table

```sql
INSERT INTO minbars2
SELECT * FROM minbars;
```

---

## Step 8.3 Verify Parquet Data

```sql
SELECT COUNT(*) FROM minbars2;
```

Expected:

```text
Record count matches source table
```

---

## Step 8.4 Compare Storage Formats

Benefits of Parquet:

* Column-oriented storage
* Better compression
* Faster analytical queries
* Reduced storage usage

---

## Screenshot

![Hive Parquet Conversion](images/hive-parquet-conversion.png)

---

# Part 9 — Screenshots

The following screenshots were captured during the execution of this lab.

## Hive Parquet Conversion

This screenshot shows successful conversion from Hive table data into 
Parquet format.

![Hive Parquet Conversion](images/hive-parquet-conversion.png)

---

# Part 10 — Troubleshooting

## Error: Output Directory Already Exists

Example:

```text
Output directory output/wordcount already exists
```

Solution:

```bash
hadoop fs -rm -r output/wordcount
```

Run the MapReduce job again.

---

## Error: Compilation Failed

Example:

```text
javac: cannot find symbol
```

Solution:

Verify:

```bash
ls WordCount.java
```

Check Java syntax and Hadoop library paths.

---

## Error: Hive Table Not Found

Example:

```text
Table not found
```

Solution:

```sql
SHOW TABLES;
```

Verify table creation before querying.

---

## Error: JSON SerDe Class Not Found

Example:

```text
ClassNotFoundException:
org.openx.data.jsonserde.JsonSerDe
```

Solution:

Verify library installation:

```bash
ls /usr/lib/hive/auxlib/
```

Check that:

```text
json-serde-1.3.8-jar-with-dependencies.jar
```

exists.

---

## Error: HDFS Path Does Not Exist

Example:

```text
No such file or directory
```

Solution:

```bash
hadoop fs -ls /
```

Verify directory names before uploading files.

---

# Part 11 — Conclusion

In this lab, we learned how to:

* Build and execute Hadoop MapReduce applications
* Monitor jobs using YARN
* Process data using Apache Pig
* Query structured data using Hive
* Read JSON data using Hive JSON SerDe
* Process compressed GZIP files
* Convert datasets into Parquet format

These technologies are fundamental components of the Hadoop ecosystem and 
are widely used in large-scale data engineering environments.

---

# Screenshots Required

Capture the following screenshots when performing the lab:

* YARN Resource Manager
* MapReduce WordCount Result
* Pig WordCount Result
* Hive Query Result
* JSON Query Result
* Parquet Conversion Result

---

# Author

Vikhom Manpiriya

Student ID: 66102010185

