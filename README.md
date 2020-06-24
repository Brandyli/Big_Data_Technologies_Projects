# Analyzing millions of NYC Parking Violations
### Introduction
For this project, you will be tasked with loading and then analyzing a dataset containing millions of NYC parking violations since January 2016. In completing this exercise, you will demonstrate mastery of principles of containerization, terminal navigation, python scripting, artifact deployment and AWS EC2 provisioning.

## Part 1: Python Script
#### The Goal
Ensure the python scripts can be run in a local machine, AWS Cloud and docker-supporting environment.
#### Task
Develop the python command line interface that connect API and demonstrate that data can be accessed via python

In the first part, you simply want to develop a python command line interface that can connect to the OPCV API and demonstrate that the data is accessible via python.
Your script must be able to run within docker but take parameters from the command line. It should also support having the option to print results out to a file.
Inputs/Outputs
Here are all the command line arguments your script must support:
```$ docker run -e APP_KEY={YOUR_APP_KEY} -t bigdata1:1.0 python main.py --page_size=1000 --num_pages=4 --output=results.json```

#### Some key arguments here:
 APP_KEY: This is how a user can pass along an APP_KEY for the api in a safe manner. DO NOT COMMIT YOUR (actual) APP KEY to Github! Also, APP_KEY should not be “hardcoded” anywhere in your source code.
bigdata1: This is the name of your docker image. We want the output for Part 1 to be version 1.
--page_size: This command line argument is required. It will ask for how many records to request from the API per call.
--num_pages: This command line argument is optional. If not provided, your script should continue requesting data until the entirety of the content has been exhausted. If this argument is provided, continue querying for data num_pages times.
--output: This command line argument is optional. If not provided, your script should simply print results to stdout. If provided, your script should write the data to the file (in this case, results.json).

It is expected that stdout or results.json will contain the API response, which is simply rows and rows of data from the API within the confines of the parameters provided to the script.

#### Libraries
For this script, you must use PyPI’s sodapy module. This module makes it seamless to connect to your Socrata API and provides a high level interface for providing additional parameters such as page offsets.
Your project should have a src folder containing functions for interacting with the OPCV API and for managing the API response. Your main.py script should live in the root folder at the same level as your Dockerfile and requirements.txt

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

## Part 2: Loading into ElasticSearch

In this second part, you want leverage docker-compose to bring up a service that encapsulates your bigdata1 container and an  elasticsearch container and ensures that they are able to interact. 
You must update your original script (from Part 1) to now not only download the data but also load it into your elasticsearch instance.
Inputs/Outputs
Same inputs as Part 1, however you must demonstrate that your script now has the ability to push the API results into elasticsearch.
In order to do this, it is sufficient to simply run a few curl requests in terminal against http://localhost:9200.

#### Libraries
In order to interact with elasticsearch in python, PyPI’s elasticsearch module ought to be used. Additionally, all elasticsearch related logic should be wrapped into an internal module that lives within the src folder in your project.

## Part 3: Visualizing and Analysis on Kibana
In this third part, you want to stand up an instance of Kibana on top of your ElasticSearch instance in order to visualize and analyze your dataset.
There will be an update required in your script - in order to properly index information on kibana, we will want to properly define a time field. The OPCV dataset does contain an issue_date  field that would be a good candidate for this definition. As part of this exercise, come up with a way to parse this field - which is stored as text - into a python datetime field.

#### Inputs/Outputs
No real difference in inputs. The only code change required is to ensure that the issue_date that is being loaded into elasticsearch is transformed into a datetime before load. 
As far as output goes, configure Kibana to pull items from the index defined when loading data into elasticsearch. This should load up the resulting data into the Kibana API and allow you to do some interesting analysis. 
Have some fun with this and try to come up with a unique analysis as you explore the capabilities of Kibana - some questions to consider answering (in the form of visualizations or graphs):
* Which county had the highest average reduction amount?
* Which violation was most popular? Second most popular? Etc
Create 4 visualizations in Kibana that analyze the data loaded and presents analysis in graphical form. Here is the inspiration for this.


