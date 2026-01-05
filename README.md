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
