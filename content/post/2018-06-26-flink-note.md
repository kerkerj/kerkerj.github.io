---
title: "Apache Flink 學習筆記"
date: 2018-06-26
slug: "flink-note"
tags: ['flink', 'scala', 'hadoop', 'docker', 'intellij', 'sbt', '大數據']
description: "Apache Flink getting started notes covering installation and testing on Mac, deploying with Docker Compose, and developing Flink jobs in Scala using IntelliJ."

---

flink, hadoop, scala

on mac

## Install flink on mac

Whatever the way you choose to install flink, download the flink directly will make things easier.

Download page: https://archive.apache.org/dist/flink/flink-1.4.2/

I download: https://archive.apache.org/dist/flink/flink-1.4.2/flink-1.4.2-bin-hadoop27-scala_2.11.tgz

I'll use flink 1.4.2, and the latest version is 1.5.0.

1. homebrew

   ```shell
   $ brew install apache-flink # flink-1.5.0
   ```

2. docker-compose 

   ref: [flink#running-a-cluster-using-docker-compose](https://github.com/docker-library/docs/tree/master/flink#running-a-cluster-using-docker-compose)

   ```dockerfile
   version: "2.1"
   services:
     jobmanager:
       image: ${FLINK_DOCKER_IMAGE_NAME:-flink}
       expose:
         - "6123"
       ports:
         - "8081:8081"
       command: jobmanager
       environment:
         - JOB_MANAGER_RPC_ADDRESS=jobmanager

     taskmanager:
       image: ${FLINK_DOCKER_IMAGE_NAME:-flink}
       expose:
         - "6121"
         - "6122"
       depends_on:
         - jobmanager
       command: taskmanager
       links:
         - "jobmanager:jobmanager"
       environment:
         - JOB_MANAGER_RPC_ADDRESS=jobmanager
   ```

   ```shell
   $ FLINK_DOCKER_IMAGE_NAME=flink:1.4.2 docker-compose up
   $ docker-compose scale taskmanager=<N>
   ```

   

## Test flink installation

1. homebrew

   Use `brew info apache-flink` to get the installation path of flink.

   e.g. `/usr/local/Cellar/apache-flink/1.5.0`, then:

   ```shell
   # terminal tab1
   # run flink
   # You can use ./libexec/bin/stop-cluster.sh to stop service
   $ cd /usr/local/Cellar/apache-flink/1.5.0
   $ ./libexec/bin/start-cluster.sh # run flink in daemon mode
   Starting cluster.
   Starting standalonesession daemon on host xxx.
   Starting taskexecutor daemon on host xxx.
   $ tail -f ./libexec/log/*.out # tail task output log

   # terminal tab2
   # open a local socket server for port 9000
   $ nc -l 9000 

   # terminal tab3
   # run flink example
   $ cd /usr/local/Cellar/apache-flink/1.5.0
   $ ./bin/flink run examples/streaming/SocketWindowWordCount.jar --port 9000
   ```

   Type some words in terminal tab2, hit enter, and terminal tab1 will show the task logs.

2. docker-compose

   ```shell
   # terminal tab1
   # run flink
   $ FLINK_DOCKER_IMAGE_NAME=flink:1.4.2 docker-compose up

   # terminal tab2
   # open a local socket server for port 9000
   $ nc -l 9000 
   ```

   Upload `examples/streaming/SocketWindowWordCount.jar` to flink, the file is in `flink-1.4.2-bin-hadoop27-scala_2.11.tgz`.

   Open http://localhost:8081/#/submit, and click the checkbox on the uploaded item.

   Insert `Program Arguments` with `--hostname docker.for.mac.localhost --port 9000`, the task will be executed.

   Type some words in terminal tab2, hit enter, and terminal tab1 will show the task logs.

   Note that the domain name `docker.for.mac.localhost` makes docker containers able to connect host.



## Develop flink tasks in Scala with Intellij

Intellij, sbt



### Intellij

Flink recommend to use [Intellij](https://www.jetbrains.com/idea/) for developing tasks in Scala, so download and install it.

Open preference (`cmd + ,`), select Plugin in sidebar, and search Scala plugin, install it.



### sbt - scala build tool

```shell
$ brew install sbt
```



### Project template

Tutorial: https://ci.apache.org/projects/flink/flink-docs-master/quickstart/scala_api_quickstart.html

```shell
$ bash <(curl https://flink.apache.org/q/sbt-quickstart.sh)
Project name (Flink Project): flink-test
Organization (org.example): org.example
Version (0.1-SNAPSHOT):
Scala version (2.11.12):
Flink version (1.5.0): 1.4.2

-----------------------------------------------
Project Name: flink-test
Organization: org.example
Version: 0.1-SNAPSHOT
Scala version: 2.11.12
Flink version: 1.4.2
-----------------------------------------------
Create Project? (Y/n):
Creating Flink project under flink-test
```

This script will help you to create project.

1. open Intellij -> import project -> select flink-test folder.

2. choose `import project from external model`, and select `sbt`

3. use java 1.8 as project SDK (or any Java version in your local machine)

4. click Finish

5. Intellij will start to dump project structure from sbt, so wait for it.

6. open `WordCount.scala`

7. Run `WordCount.scala`

   ```
   Error: Could not find or load main class org.example.WordCount
   ```

8. File -> Project Structure (or `cmd + ;`) -> library -> add -> select `java` -> open `flink-1.4.2-bin-hadoop27-scala_2.11.tgz` and select all jar files in `lib`, and select `root` module to import.

   ![image](/images/droplr/e8QOHl.png)

   ![image](/images/droplr/8oXvTk.png)

9. Run `WordCount` again, output should be like this:

   ```
   (and,1)
   (arrows,1)
   (be,2)
   (is,1)
   (nobler,1)
   (of,2)
   (a,1)
   (in,1)
   (mind,1)
   (or,2)
   (slings,1)
   (suffer,1)
   (against,1)
   (arms,1)
   (not,1)
   (outrageous,1)
   (sea,1)
   (the,3)
   (tis,1)
   (troubles,1)
   (whether,1)
   (fortune,1)
   (question,1)
   (take,1)
   (that,1)
   (to,4)

   Process finished with exit code 0
   ```



### Build quickstart project and send to flink web UI

Now let's build quickstart project.

#### Use Intellij

1. Open project structure (`cmd + ;`) and choose `Artifacts`

2. Click `+` (Add)

3. Choose `Jar` -> `from modules with dependencies ` -> click OK with default value

4. Click OK with default value (Or you can edit name and output directory)

5. Choose `Buiild` -> `Build Artifacts` to build jar file, and output file will show up in the output directory after compiling.

   ![image](/images/droplr/thI3BI.png)

#### Use sbt

```shell
$ cd /to/projects/flink-quickstart
$ sbt assembly
[info] Loading settings from idea.sbt ...
[info] Loading global plugins from /Users/kerkerj/.sbt/1.0/plugins
[info] Loading settings from assembly.sbt ...
[info] Loading project definition from /Users/kerkerj/projects/flink-quickstart/project
[info] Loading settings from idea.sbt,build.sbt ...
[info] Set current project to Flink Project (in build file:/Users/kerkerj/projects/flink-quickstart/)
[info] Compiling 3 Scala sources to /Users/kerkerj/projects/flink-quickstart/target/scala-2.11/classes ...
[warn] there was one feature warning; re-run with -feature for details
[warn] one warning found
[info] Done compiling.
[warn] Multiple main classes detected.  Run 'show discoveredMainClasses' to see the list
[info] Checking every *.class/*.jar file's SHA-1.
[info] Merging files...
[info] SHA-1: b9026fa18e6d7e9a8e8669cb7876286c9b4fa598
[info] Packaging /to/projects/flink-quickstart/target/scala-2.11/Flink Project-assembly-0.1-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 5 s, completed Jun 26, 2018 3:29:19 PM
```

The output file will be generated.

#### Run jar file in flink

1. Open http://localhost:8081/#submit

2. Upload the generated jar file, and check the uploaded item.

   ![image](/images/droplr/nieY8B.png)



Ref:

[What is the `mainRunner` project for?](https://github.com/tillrohrmann/flink-project/issues/4)

[flink-project](https://github.com/tillrohrmann/flink-project)
