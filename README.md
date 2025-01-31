# Data Intensive Computing - ID2221
This course consists of four lab assignments that span over different topics of the course: 

* **Lab 1:** In the first lab assignment you will practice the basics of the data intensive programming by setting up HDFS and Hadoop MapReduce and implementing a simple application on them.
* **Lab 2:** The second lab assignment is based of the Spark and Spark SQL, and you will learn how to use them to process and analyze data.
* **Lab 3:** The concentration of this lab assignment is to process streaming data. You will work with Spark Streaming in this lab to process data on-line as it flows.
* **Lab 4:** In the last assignment you will work with GraphX to process graph-based data.


## Lab Assignments Environment
We have used the IPython/Jupyter notebooks for the lab assignments. Notebooks are documents that contain both the programming code (e.g., python or scala), as well as human-readable text elements (e.g., paragraph, figures, and links). Since we are using Scala programming language and Spark platform in our assignments, in addition to IPthon/Jupyter, we also need to install ISpark. Below, we first explain how to install IPython/Jupyter, and then present the ISpark installation steps. Note that these steps are given for a Linux operating system, so if you do not have Linux, you need to install it either on your machine or on a VirtualBox. You can download VirtualBox from its [page](https://www.virtualbox.org). You can also find different ready to use Linux distribution images for VirtualBox [here](http://www.osboxes.org/ubuntu).

### IPython Installation
The steps to install IPython/Jupyter:

1. Install `pip`, the package management system used to install and manage software packages written in Python.
   ```bash
sudo apt-get install python-dev libncurses-dev python-pip
   ```

2. Use `pip` to install IPython. The following command downloads and installs IPython and its main optional dependencies for the notebook, qtconsole, tests, and other functionality.
   ```bash
sudo pip install ipython[all]==3.2.1
   ```

3. Download and install Java and Spark on your machine. The easiest way to install Spark is to download its "pre-built for CDH 4" from the [here](http://spark.apache.org/downloads.html).

4. Set the environment variables.
   ```bash
export JAVA_HOME=<JAVA_PATH>
export SPARK_HOME=<SPARK_PATH>
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.8.2.1-src.zip:$SPARK_HOME/python
export PYSPARK_SUBMIT_ARGS="--master local[2] pyspark-shell"
   ```

5. Create an IPython profile to support Python programming language.
   ```bash
ipython profile create pyspark
   ```

6. Edit the created profile by copying the following lines in `00-pyspark-setup.py`. This file is at `~/.ipython/profile_pyspark/startup/00-pyspark-setup.py`.
   ```python
import os
import sys
spark_home = os.environ.get('SPARK_HOME', None)
sys.path.insert(0, os.path.join(spark_home, 'python'))
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.8.2.1-src.zip'))
execfile(os.path.join(spark_home, 'python/pyspark/shell.py'))
   ```

7. Run and test the IPython notebook by running the following command.
   ```bash
ipython notebook --profile=pyspark --debug
   ```

### ISpark Installation
ISpark is an Apache Spark-shell backend for IPython.

1. Install Maven first, if you do not have it.
   ```bash
sudo apt-get install maven
   ```

2. Download ISpark from [here](https://github.com/tribbloid/ISpark/archive/master.zip).
  
3. ISpark needs to be compiled and packaged into a jar by Maven before being submitted and deployed. Extract the ISpark file you downloaded and run the following command.
   ```bash
cd ISpark-master
./mvn-install.sh
   ```
4. Create an ISpark profile.
   ```bash
ipython profile create spark
   ``` 

5. Edit the profile by copying the following lines in `ipython_config.py`, which is at `~/.ipython/profile_spark/ipython_config.py`. Below we are referring to `ispark-core-assembly-0.2.0-SNAPSHOT.jar`, which is built in step 3. It is located at `ISpark-master/core/target/scala-2.10`.
   ```python
import os
c = get_config()
spark_home = os.environ['SPARK_HOME']
master = 'local[2]'
c.KernelManager.kernel_cmd = [spark_home+"/bin/spark-submit", 
  "--master", master,
  "--class", "org.tribbloid.ispark.Main",
  "--executor-memory", "2G",
  "<PATH ON YOUR MACIHE>/ispark-core-assembly-0.2.0-SNAPSHOT.jar",
  "--profile", "{connection_file}", 
  "--parent"]
   ```

6. Run and test the notebook.
   ```bash
ipython notebook --profile=spark --debug
   ```

## Things To Deliver
The assignments can be done individually or in group of two. For the labs 1 and 3, you should hand in the implemented codes in zip files, and for the other labs, you just need to deliver the completed IPython files.

