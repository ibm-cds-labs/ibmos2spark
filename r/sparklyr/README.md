# ibmos2sparklyr

The package sets Spark Hadoop configurations for connecting to 
IBM Bluemix Object Storage and Softlayer Account Object Storage instances
with the swift protocol. This packages uses the new [swift2d/stocator](https://github.com/SparkTC/stocator) protocol, availble
on the latest IBM Spark Service instances (and through IBM Data Science Experience). 

Note, this package configures a SparkContext instantiated by SparkR and is appropriate for use
with IBM R Notebooks only. It does not support SparkContexts returned from sparklyr::spark_connect, and
can therefore not be used in IBM DSX RStudio sessions. Support for RStudio will come in the near future. 


## Installation 
    library(devtools)
    devtools::install_url("https://github.com/ibm-cds-labs/ibmos2spark/archive/adding_r_scala_platform.zip", subdir= "r/sparklyr/",dependencies = FALSE)
    
In Data Science Experience please be sure to include the "dependencies = FALSE" flag in your installation. If you forget to do this and DSX's special flavor of sparklyr gets overwritten, just go into your local R repo store ("/home/rstudio/R/x86_64-redhat-linux-gnu-library/RVERSION/") where RVERSION is the newest install of R (currently 3.3) and delete the sparklyr folder. After deleting choose File->Quit Session to refresh your R kernel. These steps will refresh your sparklyr package to the special Data Science Experience version. 

## Usage

### Bluemix

    library(ibmos2sparklyr)
    configurationname = "bluemixOScon" #can be any any name you like (allows for multiple configurations)

    # In DSX notebooks, the "insert to code" will insert this credentials list for you
    creds = list(
            auth_url="https://identity.open.softlayer.com",
            region="dallas", 
            project_id = "XXXXX", 
            user_id="XXXXX", 
            password="XXXXX")
            
    bmconfig = bluemix(sparkcontext=sc, name=configurationname, credentials = creds)
           
    container = "my_container" # name of your object store container
    object = "my_data.csv" # name of object that you want to retrieve in the container
    spark_object_name = "dataFromSwift" # name to assign to the new spark object
    
    data = sparklyr::spark_read_csv(sc, spark_object_name,bmconfig$url(container,object))


### Softlayer

    library(ibmos2sparklyr)
    configurationname = "softlayerOScon" #can be any any name you like (allows for multiple configurations)

    slconfig = softlayer(sparkcontext=sc, 
                 name=configurationname, 
                 auth_url="https://identity.open.softlayer.com",
                 tenant = "XXXXX", 
                 username="XXXXX", 
                 password="XXXXX"
           )
           
    container = "my_container" # name of your object store container
    object = "my_data.csv" # name of object that you want to retrieve in the container
    spark_object_name = "dataFromSwift" # name to assign to the new spark object

    data = sparklyr::spark_read_csv(sc, spark_object_name,slconfig$url(container,object))



## License 

Copyright 2016 IBM Cloud Data Services

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
