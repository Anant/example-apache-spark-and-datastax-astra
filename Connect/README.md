# Connect Apache Spark and DataStax Astra through Spark-Shell

We will be using [Gitpod](https://www.gitpod.io/) as our dev environment so that you can quickly test this out without having to worry about OS inconsistencies. If you have not already opened this in gitpod, then `CTR + Click` the button below and get started! <br></br>
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/Anant/example-Apache-Spark-and-DataStax-Astra)


## Step 1: Download Apache Spark 3.0.1
1. `curl -L -s https://apache.osuosl.org/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz | tar xvz -C /workspace/example-Apache-Spark-and-DataStax-Astra/Connect`

## Step 2: Run Apache Spark in standalone cluster mode with 1 worker
1. If not already: `cd Connect`
2. `cd spark-3.0.1-bin-hadoop2.7`
3. `./sbin/start-master.sh`
4. `./sbin/start-slave.sh <master-url>`
    - To the find the master url in Gitpod, open port `8080` in a new browser and copy and paste the master url into the designated spot

## Step 3: Download DataStax Astra Secure Connect Bundle and Insert into Gitpod
1. If you do not already have a DataStax Astra database, you can follow the set-up instructions on the README.md for https://github.com/Anant/cassandra.api up to Step 1.4 or "Download Secure Connect Bundle".
2. Once your database is active, connect to it, and click the Settings menu tab. Select Admin User for role and hit generate token. COPY DOWN YOUR CLIENT ID AND CLIENT SECRET as they will be used by Spark
3. Once you have downloaded the Astra Secure Connect Bundle, drag-and-drop it into the "Connect" directory in Gitpod.
  
## Step 4: Download DataStax Studio Notebook and Run Cells in Astra Studio
1. While we have DataStax Astra open, let's go ahead and download this notebook [file](https://drive.google.com/file/d/1uwCuUkd4EOi16FSCxT2gKRdhBXxtObh8/view?usp=sharing). Once the notebook is downloaded, open Studio on DataStax Astra and then drag-and-drop the notebook file into Studio.
2. Run the cells in the notebook and set up our Astra database for when we connect to it using Apache Spark. Don't forget to select your keyspace when running the cells.
 
## Step 5: Run Spark-Shell and connect to our Astra database
1. If not already: `cd spark-3.0.1-bin-hadoop2.7`
2. Insert your specific Astra database name into the `{your-db-name}` spots; as well as, insert the `{master-url}` for the master url. To the find the master url in Gitpod, open port `8080` in a new browser and copy and paste it into the designated spot below. Additionally, insert your crendentials at the designated username and password spots.
~~~
./bin/spark-shell --packages com.datastax.spark:spark-cassandra-connector_2.12:3.0.0 \
--master {master-url} \
--files /workspace/example-Apache-Spark-and-DataStax-Astra/Connect/secure-connect-{your-db-name}.zip \
--conf spark.cassandra.connection.config.cloud.path=secure-connect-{your-db-name}.zip \
--conf spark.cassandra.auth.username={Client ID} \
--conf spark.cassandra.auth.password={Client Secret} \
--conf spark.sql.extensions=com.datastax.spark.connector.CassandraSparkExtensions
~~~
  
## Step 6: Test if Spark Connected to DataStax Astra
1. You can run one or both, within the Spark-Shell to test the connection:
~~~
import org.apache.spark.sql.cassandra._
val data = spark.read.cassandraFormat("leaves", "{your-keyspace}").load
data.printSchema
data.show
~~~
AND / OR
~~~
spark.conf.set(s"spark.sql.catalog.mycatalog", "com.datastax.spark.connector.datasource.CassandraCatalog")
spark.sql("SHOW TABLES FROM mycatalog.{your-keyspace};").show
spark.sql("use mycatalog.{your-keyspace};")
spark.sql("select * from leaves").show
~~~

## Additional Resources

- If you want to watch how to do this in a live demonstration, check out this [YouTube Video](https://youtu.be/sCh07TeUWpk)
- Check out this accompanying [blog](https://blog.anant.us/connect-apache-spark-and-datastax-astra/) as well!
