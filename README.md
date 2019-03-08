# Project Batcomputer
Project Batcomputer is a working example of DevOps applied to machine learning and the field of AI

Motivations for this project:
- Understand the challenges in operationisation of ML models
- Attempt to make a reality of “DevOps for AI” 
- Integration of "closed box” processes (Azure ML Service) with real DevOps approach


**💬 Why "Project Batcomputer"?**  
The main model trained and used as the foundation of the project is based on crime data, and predictions of outcomes of crimes (convictions etc). The [Batman Batcomputer](https://en.wikipedia.org/wiki/Batcomputer) seemed like a fun way to make using such a prediction model more interesting. 

![Batcomputer in the 60s Batman TV show](docs/bc-batcave.jpg){: .framed}

# !!! 🔥 THIS REPO IS UNDERGOING A MAJOR REWRITE 🔥 !!! 
## Excuse the dust while things are being re-written to use Azure ML and remove DataBricks

Some of the main themes that make up the project:
- Wrapper app that allows the model to be run as a RESTful web API
- Continuous integration with *Azure Pipelines*
- Use of Azure ML Service and Python SDK
- Training Python notebooks that carry out the machine learning using Scikit-Learn 
- Infrastructure as code deployments into Azure
- Use of containers and Kubernetes

## Core Building Blocks
This shows a high level view of the core functional aspects of the project
![High level project building blocks](docs/high-level.png){: .framed .padded}

## Model Registry
THIS NEEDS REWRITING NOW

**💬 Why the extra files & complexity?**  
It was a design goal of the project not to present a dumb wrapper around the scoring function i.e. `model.predict_proba(features)` where a raw array of feature numbers is the expected input to the API. The `lookup.pkl` file provides a means for the developer working on training the model, to pass the encoded labels to the app and from there a more human friendly API can be exposed. And `flags.pkl` is used correspondingly to providing meaningful names for the results/scores

## Project Index
As there are a significant number of components, interactions & products involved in this project. An attempt has been made to break the things into three main sections, and to make those sections as standalone as possible:

- [Model training & machine learning in DataBricks](#machine-learning--training)
- [Wrapping the model in an API service](#model-api-service--wrapper-app)
- [DevOps CI/CD automation & pipelines](#devops-cicd)

## Repo Structure
The project doesn't represent a single codebase, there are multiple sets of artifacts, configuration files and sourcecode held here. The top level folders are as follows:
```
/aml         - Azure ML Service orchestration scripts (Python)
/assets      - Art and stuff
/azure       - Azure ARM templates
/batclient   - Frontend web client of Batcomputer to demo the model API
/data        - Source training data
/docs        - Documentation & guides 
/kubernetes  - Kubernetes configurations
 └ helm      - Helm charts to deploy the wrapper API into Kubernetes
/model-api   - Source for Python model wrapper API 
/training    - Training Python notebooks
/pipelines   - Azure DevOps pipelines 
```

## Working With This Project
**⚡ Important!**  
It is strongly advised to fork this repo to your own GitHub account before proceeding, and then clone it.  
For the DevOps CI flows this is a requirement as webhooks and other git configuration is required

## Presentation Deck
REMOVED PENDING UPDATE

---

# Machine Learning & Training
The primary focus of this project is on the operationisation aspects of machine learning, rather than the actual machine learning and the models themselves. In fact from the perspective of the model-api app and the CI/CD deployment flows the quality of the model and how it was trained & created is irrelevant

Two ML use cases are provided; one for Batcomputer (based on the crime data described above) and one for the well known "would you survive the Titanic?" used in many ML training examples

The scripts for training can either be run locally, or run within Azure ML Service as a experiment

**⚡ Important!**  
The provided code has been written by someone learning ML and trying it for the first time. It was not developed by a data scientist or someone with a background in AI. It does not represent any sort of best practice or optimal way of training a ML model with Scikit/Python or analyzing the data. However it is functional, and the resulting models serves the purposes of this project adequately 

If your main interest is in the ML and training side of things, I suggest you look elsewhere, there are thousands of excellent resources available on this topic

## Technology Stack
- [Azure DataBricks](https://docs.microsoft.com/en-gb/azure/machine-learning/service/)
- [Python 3]((https://www.python.org/))
- [Scikit-Learn](https://scikit-learn.org/stable/)
- [Pandas](https://pandas.pydata.org/)

## Full Documentation

#### [📃 Python Training Scripts](/training)

---

# Model API service / Wrapper App
The model API wrapper is a Python Flask app, designed to wrap the model with a REST based API. It is standalone, lightweight and designed to run in a container

## Technology Stack
![Model API technology stack](docs/api-stack.png){: .framed .padded}

## Full Documentation
#### [📃 Model API service - Full Docs](/model-api)

---

# DevOps CI/CD

![End to end DevOps flow](docs/devops-flow.png){: .framed .padded}

## Azure DevOps Pipelines
Azure Pipelines (part of Azure DevOps) is used to provide CI/CD automation. These carry out the Docker build of the model API image and also integrates with DataBricks for running the training jobs via a CI trigger
#### [📃 DevOps Pipelines - Full Docs](/pipelines)

## Infrastructure as Code

This Helm chart will deploy the wrapper model API app and configure a Kubernetes Ingress to route traffic to it.
#### [📃 Helm Chart - Full Docs](/kubernetes/helm)

ARM Template(s) for standing up the wrapper API app using Azure Container Instances
#### [📃 ARM Templates - Full Docs](/azure)
