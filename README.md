# BigDataProject1
# Introduction
For this project, I will load and then analyze a dataset containing millions of NYC parking violations since January 2016. In completing this exercise, I will demonstrate mastery of principles of containerization, terminal navigation, python scripting, artifact deployment and AWS EC2 provisioning.

## Part 1: Python Script
### The Goal
Ensure the python scripts can be run in a local machine, AWS Cloud and docker-supporting environment.
### Task
Develop the python command line interface that connect API and demonstrate that data can be accessed via python

* ```main.py```
```
main.py is used to parse the arguments --page_size,--num_pages,--output into api.py for function call.



* ```api.py```
```
api.py defines the functions and handle possible errors.
Some key ingredients include domain, data id, app key clients. 

# Docker Build:
- docker build -t bigdata1:1.0 .
- docker run -v $(pwd):/app APP_KEY=gvdrKod7ZgYAkYsLpZjWkinex -it bigdata1:1.0 /bin/bash
- docker run -v $(pwd):/app -e APP_KEY=gvdrKod7ZgYAkYsLpZjWkinex -it bigdata1:1.0 python -m main --page_size=2 --num_pages=5 --output =results.json

# Docker hub:
- docker login --username=brandy1103
- docker build -t bigdata1:1.0 .
- docker images | grep bigdata1
- docker tag b2e027ab4ff3
- docker tag b2e027ab4ff3 brandy1103/bigdata1:1.0
- docker push brandy1103/bigdata1:1.0
