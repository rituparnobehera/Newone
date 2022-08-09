Aim is to solve all the tasks of data engineering test.

This project is a **Spark-Scala** application.

## <u>Approach and Data Exploration</u>

### Preparing the Input Dataset:
1. Read input json as spark dataframe
2. Remove duplicate/redundant records if any
3. Here, our most important feature is **cookTime**, **prepTime** and **ingredients**. Hence, check for null/na values in these two field.
4. Drop the row if any of the above two feature (**cookTime**, **prepTime**) is corrupt.
5. Select only those columns which is required, Here we only need two columns **cookTime** and **prepTime**.

### Pre-Processing the Data:
1. Convert the ISO time format field (**cookTime**, **prepTime**) to durations in second.
2. Also make sure to check if the ISO time format field starts with **'PT'**
3. Convert the next important feature i.e **ingredients** to lower case for ease of pattern matching.

### Assumptions
1. **cookTime**, **prepTime** fields can have null and empty values.
2. **ingredients** doesn't matter if its null, empty or corrupt
3. output schema:
   1. **difficulty** : _String_
   2. **avg_total_cooking_time** : _String (ISO Time Format)_

### Calculation of average cooking time and it's difficulty level
1. Extract records for which **ingredients** contains **beef**.
2. Calculate **total_cooking_time** and it's corresponding difficulty level.
3. Find the average using spark aggregation functions.

## <u>Getting started</u>
To run the project we need to define **application.conf** file

```
test{
    spark {
        "spark.app.name" = "hellofreshdevtests"
        "spark.master" = "local[*]"
        "spark.submit.deployMode" = "client"
    }
    input {
        path = "input/"
    }
    output {
        path = "output/"
    }
}

prod{
    spark {
            "spark.app.name" = "hellofreshdevtests"
            "spark.master" = "yarn"
            "spark.submit.deployMode" = "cluster"
        }
    input {
        path = "input/"
    }
    output {
        path = "output/"
    }
}
```

### Features:

1. You can add as many spark conf as you want, 
2. more environments can be added for example: test, dev, integration, prod.
3. The environment value and path to conf can be passed as an optional argument via command line argument.
4. By Default the environment is **test** and **application.conf** path is ./config/application.conf
5. It is mandatory to have application.conf ready, either in the default path or a external fully qualified path.

### Running in Standalone
To run the project in standalone mode requires:
1. Scala 2.13
2. Spark 3.x.x
3. sbt 1.2.8 (higher version would also work with WARNING)

Download bundled spark with scala: https://dlcdn.apache.org/spark/spark-3.2.2/spark-3.2.2-bin-hadoop3.2-scala2.13.tgz

#### Command to execute the JAR
Download the executable JAR from: https://drive.google.com/file/d/1yLrJ2VmOWMpBQ0uZ0KlunVHIoLYLfo_V/view?usp=sharing

```
./bin/spark-submit --class com.hellofresh.job.trackRecipeDifficulty 0.1.0-devtest-1.0.0.jar --env test --confPath "/config/application.conf"
```

_Note: --env and --confPath is optional, however best practice is to pass arguments_

### Running in Cluster
The same configurations be used to directly run on the cluster,
Provided: Requirements has to be there.


### OR Build the executable JAR Manually and Run
```
sbt clean
sbt test
sbt assemble
```

JAR path: _**target/scala-2.13**_

After the JAR is generated, follow the above-mentioned guide to run either in standalone or over cluster.


### Scalability
The application is scalable as all performance configuration can be handled externally via application.conf

### Scala DOC
you can also generate the scala doc using command below:
```
sbt doc
```

### OR

GOTO: https://github.com/hellofreshdevtests/rituparnobehera-data-engineering-test/tree/rituparno/api
and open in a browser to see code documentation


### Code coverage and Structure:
![alt text](https://github.com/hellofreshdevtests/rituparnobehera-data-engineering-test/blob/rituparno/image/code_structure.png?raw=true)

### A glimpse of scala documentaion
![alt text](https://github.com/hellofreshdevtests/rituparnobehera-data-engineering-test/blob/rituparno/image/doc.png?raw=true)

