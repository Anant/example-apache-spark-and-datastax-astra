# Run an Apache Spark job on a DataStax Astra database

We will be using [Gitpod](https://www.gitpod.io/) as our dev environment so that you can quickly test this out without having to worry about OS inconsistencies. If you have not already opened this in gitpod, then `CTR + Click` the button below and get started! <br></br>
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/adp8ke/Apache-Spark-and-DataStax-Astra)

## Step 1: Download Apache Spark 3.0.1
1. `curl -L -s https://apache.osuosl.org/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz | tar xvz -C /workspace/Apache-Spark-and-DataStax-Astra/Job`

## Step 2: Run Apache Spark in standalone cluster mode with 1 worker
1. If not already: `cd Job`
2. `cd spark-3.0.1-bin-hadoop2.7`
3. `./sbin/start-master.sh`
4. `./sbin/start-slave.sh <master-url>`
    - To the find the master url in Gitpod, open port `8080` in a new browser and copy and paste the master url into the designated spot

## Step 3: Download DataStax Astra Secure Connect Bundle and Insert into Gitpod
1. If you do not already have a DataStax Astra database, you can follow the set-up instructions on the README.md for https://github.com/Anant/cassandra.api up to Step 1.4 or "Download Secure Connect Bundle".
2. Once you have downloaded the Astra Secure Connect Bundle, drag-and-drop it into the "Job" directory in Gitpod.
  
## Step 4: Download DataStax Studio Notebook and Run Cells in Astra Studio
1. While we have DataStax Astra open, let's go ahead and download this notebook [file](). Once the notebook is downloaded, open Studio on DataStax Astra and then drag-and-drop the notebook file into Studio.
2. Run the cells in the notebook and set up our Astra database for when we connect to it using Apache Spark. Don't forget to select your keyspace when running the cells.

## Step 5: Download sbt 1.4.3
1. `curl -L -s https://github.com/sbt/sbt/releases/download/v1.4.3/sbt-1.4.3.tgz | tar xvz -C /workspace/Apache-Spark-and-DataStax-Astra/Job`

## Step 6: Start sbt server in `spark-cassandra` directory
1. `cd spark-cassandra`
2. `../sbt/bin/sbt`

## Step 7: Edit Values in `spark-cassandra/src/main/scala/spark-cassandra.scala`
1. Insert your specific configs in lines 
2. Save the file

## Step 8: Build Fat JAR
1. Run `sbt assembly` in sbt server to build jar
 
## Step 9: Run Spark-Submit and Run a Job on DataStax Astra
1. If not already: `cd spark-3.0.1-bin-hadoop2.7`
2. Insert your specific database name in the spot designated below
3. Run the block in terminal to run the Spark Job
~~~
./bin/spark-submit --class sparkCassandra.Leaves \
--files /workspace/Apache-Spark-and-DataStax-Astra/Job/secure-connect-{your-database}.zip \
/workspace/Apache-Spark-and-DataStax-Astra/Job/spark-cassandra/target/scala-2.12/spark-cassandra-assembly-0.1.0-SNAPSHOT.jar
~~~

## Additional Resources

- If you want to watch how to do this in a live demonstration, check out this [YouTube Video]()
- Check out this accompanying [blog]() as well!
