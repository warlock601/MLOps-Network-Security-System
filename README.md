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
