# MLOps-Network-Security-System
Repository for building an end-to-end MLOps solution for a Network Security system.


### Steps
- First create a virtual environment using Conda.
  ```bash
  conda create -p venv python==3.10
  ```
- Create requirements.txt which will contain all the libraries that will be used. Also create .github/wrokflows/main.yml for CI/CD workflow code & .gitignore file.

- Create the project structure which will have the heirarchy. This project strcuture is what companies generally use for their MLOps workflows.
  "notebooks" folder will consist of all the notebooks. "exception" will be used for exception handling, "logging" will be used for logging handling & "cloud" folder for anything related to cloud.


- After the entire project structure is created, activate the environment.
  ```bash
  conda activate venv/
  ```


- Added the required libraries in requirements.txt for this project
  ```bash
  python-dotenv
  pandas
  numpy
  ```
  Then all the packages can be installed using
  ```bash
  pip install -r requirements.txt"
  ```

  Inside .gitignore add these so that they dont get tracked.
  ```bash
  venv
  .env
  ```

- In all of the project structure folders given below, create __inti__.py file. Almost same structure is used in companies for MLOps workflow. </br>
  <img width="222" height="200" alt="image" src="https://github.com/user-attachments/assets/27c492de-7526-4b80-b755-af6f6bd3e960" />


- Create setup.py. The setup.py file is an essential part of packaging and distributing python projects. It is used by setuptools (or distutils in older Python versions) to define the configuration of your project, such as its metadata, dependencies, and more. Main aim is to setup Metadata. </br>
  __find_packages__ is a function that scans through all the folders and wherever there is an __inti__.py file, it is going to consider that particular folder as a package. Like it will consider "networksecurity" as a package. Inside a package there can be multiple packages.</br>
  __setup__ is responsible for providing all the info regarding the projects.</br>
  </br>
  Whenever we are creating our entire python package, it is necessary to install all the requirements. For this we'll create a function "get_requirements"
  ```bash
  from setuptools import find_packages,setup
  from typing import List

  def get_requirements()->List[str]:
    """
    This func will rweturn list of requirements

    """

    requirement_list:List[str]=[]
    try:
        with open('requirements.txt','r') as file:
            # Read lines from the file
            lines=file.readlines()
            ## Process each line
            for line in lines:
                requirement=line.strip()
                ## ignore empty lines and -e.
                if requirement and requirement!='-e.':
                    requirement_list.append(requirement)
    except FileNotFoundError:
        print("requirements.txt file not found")
    
    return requirement_list

  print(get_requirements())

  setup(
    name="NetworkSecurity",
    version="0.0.1",
    author="Vivek Raj",
    author_email="vr32288gmail.com",
    packages=find_packages(),
    install_requires=get_requirements()
  )
  ```
  Now when we run "python setup.py" in Terminal, this will return all the libraries that are in requirements.
  <img width="504" height="30" alt="image" src="https://github.com/user-attachments/assets/55bac8da-d0fe-42ce-9bb4-7f9552c447ab" />
  
  Modified requirements.txt to include more libraries and "-e ."</br>
  __-e .__ is going to refer to setup.py file and when it is going to setup.py, it will execute the entire code in setup.py which will setup this entire python project as a package.
  ```bash
  python-dotenv
  pandas
  numpy
  pymongo
  certifi

  -e.
  ```

  As soon as we install using "pip install -r requirements.txt", so basically it will see that "-e ." is present so it will build the package from setup.py.
  <img width="926" height="146" alt="image" src="https://github.com/user-attachments/assets/91cb5f85-39ee-4b4a-bcd6-96b858f0ad32" /></br>
  A list of packages will get created in the project file structure.</br>
  <img width="262" height="216" alt="image" src="https://github.com/user-attachments/assets/7db04aca-b875-4f1b-98f5-b460f6483bf1" />   </br>
  ** Right now we will comment "-e." as we will require it at the end of the project.



- Inside networksecurity > logging directory, logging.py is created, and the following logic is added
  ```bash
  import logging
  import os
  from datetime import datetime

  LOG_FILE=f"{datetime.now().strftime('%m_%d_%Y_%H_%M_%S')}".log"

  logs_path=os.path.join(os.getcwd(),"logs",LOG_FILE)
  os.makedirs(logs_path,exist_ok=True)

  LOG_FILE_PATH=os.path.join(logs_path,LOG_FILE)

  logging.basicConfig(
    filename=LOG_FILE_PATH,
    format="[ %(asctime)s ] %(lineno)d %(name)s - %(levelname)s -%(message)s",
    level=logging.INFO,
  )

  ```
   Similarly in networksecurity > exception, create a file named exception.py (we'll also import networksecurity > logging > logger here) and add the following logic
```bash
import sys
from networksecurity.logging import logger

class NetworkSecurityException(Exception):
    def __init__(self,error_message,error_details:sys):
        self.error_message = error_message
        _,_,exc_tb = error_details.exc_info()
        
        self.lineno=exc_tb.tb_lineno
        self.file_name=exc_tb.tb_frame.f_code.co_filename 
    
    def __str__(self):
        return "Error occured in python script name [{0}] line number [{1}] error message [{2}]".format(
        self.file_name, self.lineno, str(self.error_message))
        
if __name__=='__main__':
    try:
        logger.logging.info("Enter the try block")
        a=1/0
        print("This will not be printed",a)
    except Exception as e:
           raise NetworkSecurityException(e,sys)
```
  After we run this exception.py, a folder will be created inside the "exception" folder which will store all the logs.
  <img width="350" height="138" alt="image" src="https://github.com/user-attachments/assets/02b584f3-c59e-4c26-b893-2473c6c65789" />

</br>
</br>

## Project Structure
<img width="1596" height="494" alt="image" src="https://github.com/user-attachments/assets/89b87541-5b9a-4868-995e-854d2eb3cd22" />

__ETL__ : The data source can be local, APIs, S3 bucket, Paid APIs, multiple sources etc. We need to combine this paticular data. This is called extraction. Now let say the data is in csv format, so we'll need to perform some basic preprocessing, clean raw data and then convert it into json format. This is called Transformation. After this we store the processesed data in the destination like MongoDB, DynamoDB, MySQL, S3 buckets etc. This is Loading.


- We will be creating an ETL Pipeline with MongoDB as destination. First create an account in MongoDB Atlas. </br>
  MongoDB Atlas is Database-as-a-Service (DBaaS) for MongoDB. MongoDB Atlas is a fully managed cloud database service provided by MongoDB that simplifies deploying, operating, and scaling MongoDB databases across major cloud providers. </br>
  As soon as we setup MongoDB, it will ask to deploy the cluster. pymongo is a library that allows connecgtion to MongoDB. 
  <img width="1764" height="742" alt="image" src="https://github.com/user-attachments/assets/19f16b06-be79-4a0f-abf4-451f77e416f4" />
  Then we create a DB user, choose connection method > Drivers, se;lect the Driver as Python and its version. Then we get a command to install the driver. Also we'll need to update the package in requirements.txt as well. If let say I selected Python as driver with verion 3.12 or later then in          requirements.txt we add
  ```bash
  pymongo[srv]==3.12
  ```
  Once the cluster is created, we see something like this:</br>
  <img width="1816" height="816" alt="image" src="https://github.com/user-attachments/assets/8f1fdb70-6f16-478f-bae0-02906c549f02" />


- We need MongoDB connection-string. In Cluster Overview > Click on Connect > Drivers > View full code sample > uri will show the connection-string.  To get the password, in Security > QuickStart > Edit the password if you don't know. Now create a file named push_data.py in the root folder with the     following logic and replace with your password.
```bash
from pymongo.mongo_client import MongoClient
from pymongo.server_api import ServerApi
uri = "mongodb+srv://vr32288_db_user:<db_password>@cluster0.hs5meio.mongodb.net/?appName=Cluster0"
# Create a new client and connect to the server
client = MongoClient(uri, server_api=ServerApi('1'))
# Send a ping to confirm a successful connection
try:
    client.admin.command('ping')
    print("Pinged your deployment. You successfully connected to MongoDB!")
except Exception as e:
    print(e)
```
  When we run this script, we'll get this: </br>
  <img width="532" height="46" alt="image" src="https://github.com/user-attachments/assets/73e10d15-37cc-4102-bc15-37df97faba3d" />


- Inside .env file, add a key MONDO_DB_URL which will have the entire string as value. Now as we've tested push_data that is pushed data to MongoDB, we remove all the code that we pasted in push_data.py and put it in a different file which will be referenced later on.


- ETL code will be put inside push_data.py. We'll import librarieslike "certifi" which provides a set of root certificates. It is used by python libraries that need to make a secure HTTP connection. where() function of certifi retreives the path to the bundle of   certificates provided by certifi and stores it in a particular variable. Like here we've used "ca". This is usually done for SSL or TLS connections to verfiy that the server you're connected to has a verified certificate. </br>
  We'll also need to implementt logging & exception, so that will also be imported.
- Let say we have a dataset and we have features like A,B,C. So how to insert this data in the form of JSON into MongoDB? In this cenario we convert every record in the form of list of JSON.
  <img width="1094" height="646" alt="image" src="https://github.com/user-attachments/assets/dc0eaf81-8351-4eb7-b6cd-a6eee2005c4a" />
```bash
import os
import sys
import json

from dotenv import load_dotenv
load_dotenv()

MONGO_DB_URL=os.getenv("MONGO_DB_URL")
print(MONGO_DB_URL)

import certifi
ca=certifi.where()

import pandas as pd
import numpy as np
import pymongo
from networksecurity.exception.exception import NetworkSecurityException
from networksecurity.logging.logger import logging

class NetworkDataExtract():
    def __init__(self):
        try:
            pass
        except Exception as e:
            raise NetworkSecurityException(e,sys)
        
    def csv_to_json_convertor(self,file_path):
        try:
            data=pd.read_csv(file_path)
            data.reset_index(drop=True,inplace=True)
            records=list(json.loads(data.T.to_json()).values())
            return records
        except Exception as e:
            raise NetworkSecurityException(e,sys)
        
    def insert_data_mongodb(self,records,database,collection):
        try:
            self.database=database
            self.collection=collection
            self.records=records

            self.mongo_client=pymongo.MongoClient(MONGO_DB_URL)
            self.database = self.mongo_client[self.database]
            
            self.collection=self.database[self.collection]
            self.collection.insert_many(self.records)
            return(len(self.records))
        except Exception as e:
            raise NetworkSecurityException(e,sys)
        
if __name__=='__main__':
    FILE_PATH="Network_Data/phisingData.csv"
    DATABASE="vr32288"
    Collection="NetworkData"
    networkobj=NetworkDataExtract()
    records=networkobj.csv_to_json_convertor(file_path=FILE_PATH)
    print(records)
    no_of_records=networkobj.insert_data_mongodb(records,DATABASE,Collection)
    print(no_of_records)
        




```
  When this script is ran, we'll get this kind of output which shows the csv data is converted to JSON.
  <img width="1314" height="410" alt="image" src="https://github.com/user-attachments/assets/cd162f46-9445-4188-b5f6-af32d06c00be" />



