# ShellFusion Tool Readme
This is a tool demo demonstration of our work ShellFusion. In this README, we provide a walkthrough of our tool demonstration and the way to deploy the tool.
## Table of Contents

- [ShellFusion Tool Readme](#shellfusion-tool-readme)
  - [Table of Contents](#table-of-contents)
  - [Introduction of ShellFusion](#introduction-of-shellfusion)
    - [Backend](#backend)
    - [Frontend User Interface](#frontend-user-interface)
  - [Tool Deployment](#tool-deployment)
    - [Step 1: Start Elasticsearch Using Docker.](#step-1-start-elasticsearch-using-docker)
    - [Step 2: Download Datasets and Build the Elasticsearch Index.](#step-2-download-datasets-and-build-the-elasticsearch-index)
    - [Step 3: Deploy the Frontend](#step-3-deploy-the-frontend)
    - [Step 4: Deploy the Backend](#step-4-deploy-the-backend)
  - [Usage](#usage-instruction)

## Introduction of ShellFusion
In our paper, we have implemented ShellFusion as a stand-alone web-based tool to help users address shell programming tasks by recommending relevant shell commands and presenting comprehensive answers. The tool consists of two parts: the `Backend` and the `Frontend User Interface`.
### Backend
The backend is responsible for the answer generation by leveraging the dataset we built. The dataset we provide contains:
 - Shell-Related Questions
 - the Word Embedding Model.
 - the IDF Vocabulary
 - the shell commands with options detected from the accepted answers of the questions
 - the Matrix Representations of the Questions
 - the Elasticsearch Index of the Questions

Based on the datasets, the function of the Backend includes _Similar Question Retrieval_, _Shell Command Detection, Filtering & Ranking_ and _Answer Generation_.
### Frontend User Interface
We develop the frontend of ShellFusion using the `Vue CLI 3` framework. It's responsible for obtaining the query and presenting the answer in a clean and understandable way.
## Tool Deployment
It takes four steps to deploy the tool. 
### Step 1: Start Elasticsearch Using Docker.

The first step is to start an Elasticsearch service using Docker. We release the Elasticsearch configuration file `elasticsearch.yml` in the **ShellFusion-backend** folder. The Elasticsearch service can be started by  
```sh
docker run -it --name es01 --ipc=host -p 9200:9200 -v {yourpath}/elasticsearch.yml:/config/elasticsearch.yml docker.elastic.co/ elasticsearch/elasticsearch:8.0.0
``` 
If the service starts successfully, the output will be as ![Elasticsearch](https://github.com/QuinVIVER/ShellFusion-tooldemo/blob/main/figs/fig5.jpg?raw=true) 
Note down the _password for the elastic user_ generated by the Elasticsearch service for later use. 

> Elasticsearch service requires the PC has up to 32G of ram and has a disk with more than 20% spare space, or it won't start.

### Step 2: Download Datasets and Build the Elasticsearch Index.

In this step, we first need to download the dataset of ShellFusion.
```sh
wget http://8.134.73.140:30021/exp_manual.tgz
wget http://8.134.73.140:30021/exp_models_dir.tgz
wget http://8.134.73.140:30021/exp_posts_dir.tgz
```
After downloading the tar files, unpack them into the **ShellFusion-backend** folder. 
To build the Elasticsearch index, navigate into the **ShellFusion-backend** folder, change `<yourpassword>` in line 17 of `online/ES.py` to the _password for the elastic user_, then run
``` sh
python3 online/ES.py --config config.yml
```
> if you're not deploying the Elasticsearch service at localhost, change the IP address in line 17 of `online/ES.py` to yours.
### Step 3: Deploy the Frontend
First, install `Nginx` and the `Node.js` framework, then navigate into **ShellFusion-frontend** folder and run the command below to install the frontend dependencies. 
```sh
npm install
```
After that, we generate the frontend package, move the generated package to the Nginx folder and reload Nginx by running
```sh
npm run build
mv dist /usr/share/nginx/html
nginx -s reload
```
Now we can access the homepage of ShellFusion via the link `localhost:8001`. Note that we configure Nginx to show the homepage on port 8001. The configuration file `nginx.conf` can be found in the **ShellFusion-frontend** folder.

![Tool page](https://github.com/QuinVIVER/ShellFusion-tooldemo/blob/main/figs/sf.jpg?raw=false) 

### Step 4: Deploy the Backend

First, navigate into the **ShellFusion-backend** folder, install the backend dependencies running
 ```python
 pip install -r requirements.txt
 ```
Then, deploy the backend.
```sh
python3 online/backend.py --config config.yml
```
> In practice, we recommend using screen or other approaches to ensure the persistence of the Backend.

## Usage Instruction
To use the ShellFusion tool, the user just needs to input their code-related query into the input box and click the search button, the tool will then return the generated answer.
For users to better understand the usage of the tool, we offered an example query: ***How to extract the first two characters of a string in shell scripting***. The answer for the example query is as below:
![query](https://github.com/QuinVIVER/ShellFusion-tooldemo/blob/main/figs/SFresult.jpg?raw=false) 
