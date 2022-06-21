# ShellFusion-tooldemo

This repository store the **code and data** of ShellFusion tooldemo.
## **Backend module**
The backend repository contains the backend inplemented in python and dataset used. Datasets we released include:
<br>**The entire set of shell-related quesions**
<br>**The shell commands with options detected from the accepted answers of the questions**
<br>**The matrix representations of the questions**
<br>**The Elasticsearch index of the questions, the word IDF vocabulary**
<br>**And the word embedding model.**
<br>The README file of backend repository well documented how to download the datasets and the way to deploy the backend.
## **Frontend module**
The frontend module of ShellFusion use vue-cli3 to implement, serving the propose of getting and sending user questions to the backend via websocket, and showing the answer generated by ShellFusion. More specifically, it use bootstrap to manage answer layout, use highlightjs to manage code block highlighting.
<br>The detailed tutorial on how to deploy the frontend is well documented in the README file of the frontend repository.