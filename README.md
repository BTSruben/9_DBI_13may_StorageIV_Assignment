# 9_DBI_13may_StorageIV_Assignment

1. We need to add our datase into Hadoop System

    Copy dataset to namenode container:

        docker cp inputMapReduce NAMENODE_CONTAINER_ID:/

    Login to NameNode container:

        docker exec -it NAMENODE_CONTAINER_ID bash

    Copy dataset to Hadoop System:

        hdfs dfs -copyFromLocal /inputMapReduce /

    Check if dataset appears:

        hdfs dfs -ls /inputMapReduce

2. Run a job in Hadoop System

    Copy compiled code to namenode container:

        docker cp ProductSalePerCountry.jar NAMENODE_CONTAINER_ID:/

    Launch our job in Hadoop System:

        hadoop jar /ProductSalePerCountry.jar /inputMapReduce /mapreduce_output_sales

    Show the result:

        hdfs dfs -cat /mapreduce_output_sales/part-00000


Can you avoid these steps, How to compile jar file:

    Copy code to the container:

        docker cp MapReduceTutorial NAMENODE_CONTAINER_ID:/

    Export classpath:

        export CLASSPATH="/usr/lib/hadoop/client/hadoop-mapreduce-client-core.jar:/usr/lib/hadoop/client/hadoop-mapreduce-client-common.jar:/usr/lib/hadoop/client/hadoop-mapreduce-client-common.jar:/MapReduceTutorial/SalesCountry/*:/usr/lib/hadoop/lib/*:/usr/lib/hadoop/hadoop-common.jar"

    Go to project folder:

        cd MapReduceTutorial

    Compile Java files. Its class files will be put in the package directory:

        javac -d . SalesMapper.java SalesCountryReducer.java SalesCountryDriver.java

    Create a Jar file:

        jar cfm ProductSalePerCountry.jar Manifest.txt SalesCountry/*.class


