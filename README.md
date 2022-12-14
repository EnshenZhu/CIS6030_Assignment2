# <div style="text-align: center;">CIS6030 Information System</div>

## <div style="text-align: center;">Assignment 2</div>

## <div style="text-align: center;"> Enshen Zhu (1194726)</div>

****

### General Background

Since Hadoop is extremely hard to be properly installed and configed on the Windows OS. This assignment uses the Windows
Docker Desktop to imply the Hadoop Cluster, as well as fulfilling multiple WordCount functions.

### How To Use

To imply the Hadoop Cluster on the Docker, we need to firstly install the Docker Desktop on the Windows, and then settle
the Hadoop Cluster on the Docker.

#### I. Install Docker Desktop

Please kindly review this [official installation guidance](https://docs.docker.com/desktop/install/windows-install/)

To verify the integrity of the Docker Desktop, please open the terminal and enter the following commands

```
$ docker --version
$ docker compose --version
```

You should correctly view up the version of Docker Desktop on your machine.

#### II. Set Up the Hadoop Cluster on the Docker Desktop

1. Use git to download the the Hadoop Docker files from
   the [Big Data Europe repository](https://github.com/big-data-europe/docker-hadoop)

   ```$ git clone https://github.com/big-data-europe/docker-hadoop.git```
2. Open the terminal from the folder and enter the following

   ```docker-compose up -d```

   This command will deploy the Hadoop Cluster onto the docker

3. You can also check the running status of the containers inside the docker by the following command

   ```docker ps```

#### III. Transfer the data and the java scripts (not JavaScript) into the Cluster

1. <b>Copy the A1_data.txt file <em>(RECALL: Due to the assignment requirement, the A1_data.txt file is not inside the
   submission folder. Please kindly attach the data file by yourself!)</em> and the Count_model folder to the cloned
   repo</b>
2. In the terminal, enter ```docker exec -it namenode bash``` to get into the namenode bash terminal. A similar
   interface may look like follow

   ```
   D:\Guelph_Master\CIS6030 Information System\CIS6030_Assignment\CIS6030_Assignment2>docker exec -it namenode bash
   root@56ab0ee74f48:/#
   ```
3. Create a input directory inside the namenode:/tmp by
   ```
   cd tmp
   mkdir input
   ```
4. Exit the namenode bash terminal by entering ```exit```
5. Copy the documents and data to the namenode/tmp by (Please make sure you are no longer inside the namenode bash
   terminal)

   ```
   docker cp ./Count_model/WordCount_Q1.jar namenode:/tmp/input
   docker cp ./Count_model/WordCount_Q2.jar namenode:/tmp/input
   docker cp A1_data.txt namenode:/tmp
   ```

##### IV. Sync the input data to the Hadoop HDFS

1. Get into the namenode bash by  ```docker exec -it namenode bash```
2. Get into the tmp folder by ```cd tmp```
3. Create a hdfs directory named input by ```hadoop fs -mkdir -p input```
4. Place the input files in all the datanodes on HDF by ```hdfs dfs -put ./input/* input```

### Results

#### I. Counting the numbers of strings which has the length equal or more than 25

1. make sure you are still inside the namenode bash terminal with the route of the ./tmp directory. You may review
   the <b>Part IV</b> first and second steps to re-enter into the namenode bash
2. Run the WordCount_Q1 in the namenode by

   ```
   hadoop jar WordCount_Q1.jar org.example.WordCount_Q1 input output
   ```

3. When the terminal finish the work, enter ```hdfs dfs -cat output/part-r-00000``` to view the results
4. The final results should look like as follows

   ```
   root@56ab0ee74f48:/tmp# hdfs dfs -cat ./output/part-r-00000
   2022-10-15 01:11:23,350 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
   agriculturalcommunication       1
   agriculturalcommunications      1
   compassionateconsideration      1
   contemporaryenvironmental       1
   describingextracurricular       3
   environmentalperspectives       10
   extracurricularactivities       24
   extracurricularexperience       2
   extracurricularleadership       1
   internationalbaccalaureate      1
   interpersonalcommunication      2
   outstandingextracurricular      1
   participatedsignificantly       5
   psychologicalcompassionate      2
   universityextracurricular       6
   ```
   
#### II. Counting the numbers of strings which has the length more than 25
1. make sure you are still inside the namenode bash terminal with the route of the ./tmp directory. You may review
   the <b>Part IV</b> first and second steps to re-enter into the namenode bash
2. Run the WordCount_Q1 in the namenode by

   ```
   hadoop jar WordCount_Q1_1.jar org.example.WordCount_Q1 input output1_1
   ```

3. When the terminal finish the work, enter ```hdfs dfs -cat output1_1/part-r-00000``` to view the results
4. The final results should look like as follows
   ```
   2022-10-17 16:35:39,299 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
   agriculturalcommunications      1
   compassionateconsideration      1
   internationalbaccalaureate      1
   interpersonalcommunication      2
   outstandingextracurricular      1
   psychologicalcompassionate      2
   ```

#### III. Counting the numbers of strings which has the length equal or more than 25

1. make sure you are still inside the namenode bash terminal with the route of the ./tmp directory. You may review
   the <b>How To Use - Part IV</b> first and second steps to re-enter into the namenode bash
2. Run the WordCount_Q1 in the namenode by

   ```
   hadoop jar WordCount_Q2.jar org.example.WordCount_Q2 input output2
   ```

3. When the terminal finish the work, enter ```hdfs dfs -cat output2/part-r-00000``` to view the results
4. The final results should look like as follows
   ```
   root@56ab0ee74f48:/tmp# hdfs dfs -cat ./output2/part-r-00000
   2022-10-15 01:19:08,211 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
   Total Numbers of String:        468820
   ```

### When Finished
1. Leave the namenode bash by entering ```exit```
2. Shutdown the Hadoop cluster by entering ```docker-compose down```

### Reference

1. https://cjlise.github.io/hadoop-spark/Setup-Hadoop-Cluster/
2. https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
3. https://www.youtube.com/watch?v=dLTI2HN9Ejg&t=401s