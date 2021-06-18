# Run an Apache Spark job on a DataStax Astra database

In this directory, we will be running 2 Spark jobs on our Astra database. The first Spark job will read our `leaves` table (same table from the `Connect` directory, but with a few more records), and create 2 new tables: `leaves_by_tag` and `tags`. The first Spark job will take the data from the `leaves` table and create 2 dataframes with spark.sql manipulations. Then, we will create the 2 tables mentioned above, and seed them with their respective dataframes. The second Spark job runs the same code as the first Spark job minus the creation of the tables in order to update the existing tables (`leaves_by_tag` and `tags`) with a new record that was added to the `leaves` table.

We will be using [Gitpod](https://www.gitpod.io/) as our dev environment so that you can quickly test this out without having to worry about OS inconsistencies. If you have not already opened this in gitpod, then `CTR + Click` the button below and get started! <br></br>
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/Anant/example-Apache-Spark-and-DataStax-Astra)

## Step 1: Download Apache Spark 3.0.1
1. `curl -L -s https://apache.osuosl.org/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz | tar xvz -C /workspace/example-Apache-Spark-and-DataStax-Astra/Job`

## Step 2: Run Apache Spark in standalone cluster mode with 1 worker
1. If not already: `cd Job`
2. `cd spark-3.0.1-bin-hadoop2.7`
3. `./sbin/start-master.sh`
4. `./sbin/start-slave.sh <master-url>`
    - To the find the master url in Gitpod, open port `8080` in a new browser and copy and paste the master url into the designated spot

## Step 3: Download DataStax Astra Secure Connect Bundle and Insert into Gitpod
1. If you do not already have a DataStax Astra database, you can follow the set-up instructions on the README.md for https://github.com/Anant/cassandra.api up to Step 1.4 or "Download Secure Connect Bundle".
2. Once your database is active, connect to it, and click the Settings menu tab. Select Admin User for role and hit generate token. COPY DOWN YOUR CLIENT ID AND CLIENT SECRET as they will be used by Spark
3. Once you have downloaded the Astra Secure Connect Bundle, drag-and-drop it into the "Job" directory in Gitpod.
  
## Step 4: Download DataStax Studio Notebook and Run Cells 1-3 in Astra Studio
1. While we have DataStax Astra open, let's go ahead and download this notebook [file](https://drive.google.com/file/d/1XmLF1iKc23orQkagf0L-FOdqwKZPujdF/view?usp=sharing). Once the notebook is downloaded, open Studio on DataStax Astra and then drag-and-drop the notebook file into Studio.
2. Run cells 1-3 in the notebook and set up our Astra database for when we connect to it using Apache Spark. Don't forget to select your keyspace when running the cells.

## Step 5: Download sbt 1.4.3
1. `curl -L -s https://github.com/sbt/sbt/releases/download/v1.4.3/sbt-1.4.3.tgz | tar xvz -C /workspace/example-Apache-Spark-and-DataStax-Astra/Job`

## Step 6: Start sbt server in `spark-cassandra` directory
1. Open a new terminal
2. `cd Job/spark-cassandra`
3. `../sbt/bin/sbt`

## Step 7: Edit Values in `spark-cassandra/src/main/scala/spark-cassandra.scala`
1. Insert your specific configs and credentials in lines 12-16
2. Save the file

## Step 8: Build Fat JAR
1. Run `assembly` in sbt server to build the JAR
 
## Step 9: Run First Spark-Submit Job to Create and Seed `leaves_by_tag` and `tags` tables
1. If not already: `cd spark-3.0.1-bin-hadoop2.7`
2. Insert your specific database name in the spot designated below
3. Run the block in terminal to run the Spark Job
~~~
./bin/spark-submit --class sparkCassandra.Leaves \
--files /workspace/example-Apache-Spark-and-DataStax-Astra/Job/secure-connect-{your-database}.zip \
/workspace/example-Apache-Spark-and-DataStax-Astra/Job/spark-cassandra/target/scala-2.12/spark-cassandra-assembly-0.1.0-SNAPSHOT.jar
~~~

## Step 10: See the Results of the First Spark Job on DataStax Astra Studio
1. Run cells 4-5 in the notebook to visualize the results of the Spark job

## Step 11: Add a New Record to the `leaves` table to Set Up the Second Spark Job
1. Run cell 6 in the notebook to add a new record to the `leaves` table.
2. Run cell 3 again to confirm that there are now 3 records in the `leaves` table.

## Step 12: Edit Values in `spark-cassandra/src/main/scala/update.scala`
1. Insert your specific configs and credentials in lines 12-16
2. Save the file

## Step 13: Update Fat JAR
1. Run `assembly` in sbt server to build jar
 
## Step 14: Run Second Spark-Submit Job to Update the `leaves_by_tag` and `tags` tables
1. If not already: `cd spark-3.0.1-bin-hadoop2.7`
2. Insert your specific database name in the spot designated below
3. Run the block in terminal to run the Spark Job
~~~
./bin/spark-submit --class sparkCassandra.Update \
--files /workspace/example-Apache-Spark-and-DataStax-Astra/Job/secure-connect-{your-database}.zip \
/workspace/example-Apache-Spark-and-DataStax-Astra/Job/spark-cassandra/target/scala-2.12/spark-cassandra-assembly-0.1.0-SNAPSHOT.jar
~~~

## Step 15: See the Results of the Second Spark Job on DataStax Astra Studio
1. Run cells 4-5 in the notebook to visualize the results of the second Spark job

## Additional Resources

- If you want to watch how to do this in a live demonstration, check out this [YouTube Video]()
- Check out this accompanying [blog]() as well!
