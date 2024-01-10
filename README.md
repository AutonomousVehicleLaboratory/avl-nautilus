# Nautilus

This repo maintains useful resources for Nautilus usage. The outline is as follows. 

- [Nautilus](#nautilus)
  - [Getting started](#getting-started)
    - [Setup](#setup)
    - [Configuration](#configuration)
    - [Workflow](#workflow)
  - [Datasets](#datasets)
  - [Useful resources](#useful-resources)



## Getting started 
**Please carefully read the [policy](https://docs.nationalresearchplatform.org/userdocs/start/policies/) before using the cluster. It is also recommended to read through the entire document to get started. Another useful resource to introduce Nautilus usage is [here](https://docs.google.com/presentation/d/1GMvaZr9Nm6LhYUU_E0E0LdoebPpk0dgb2Z6oS9v2Ww8/edit?usp=sharing).**


### Setup
- Log in nautilus [portal](https://portal.nrp-nautilus.io/). Please select "University of California, San Diego" as the identity provider and log in with your UCSD account.
- Join the the official support channel **Matrix** follow [this intruction](https://docs.nationalresearchplatform.org/userdocs/start/contact/). Inside Matrix, find and join **CogRob - ERL - RY Group** room using the option "Explore public rooms".
- Please contact our group admins (Luobin Wang) and provide your UCSD email account to join our namespace (cogrob).
- Follow [this instruction](https://ucsd-prp.gitlab.io/userdocs/start/quickstart/) to setup the kubernetes client.  


### Configuration 
Nautilus provides several examples [here](https://gitlab.com/dimm/prp_k8s_config). You might also start from [``pod-example.yaml``](example/pod-example.yaml) or [``job-example.yaml``](example/job-example.yaml).


### Workflow
- Claim your own/shared persistent storage.
- Find a good Docker image for your project. Build your docker image if necessary. **REQUIRED: install packages and dependencies in your images instead of PVC.**
    - You can follow this [guide](https://docs.docker.com/get-started/) to get familiar with docker.
    - [DockerHub](https://docs.docker.com/docker-hub/) and [Nautilus](https://ucsd-prp.gitlab.io/userdocs/development/gitlab/) both allow you to upload your images.
- Try training commands using a pod/deployment.
- Create and run your job.
- Monitor and set correct resource usage (required)
- Retrieve your result in persistent storage.
- Delete your deployment/job/pod (required).



## Datasets
Commonly used datasets are maintained by the namespace admin. Please check [this Google Sheet](https://docs.google.com/spreadsheets/d/1s3GpJJr73t-OxBFYunJ2Uzmk_hfJ3ObGWH3r7l8vkPQ/edit?usp=sharing) to see whether a dataset has already been downloaded. 

- **Please do not directly store processed data into the shared PVC (cogrob-avl-west-vol). Store them in your own PVC instead.**
- **Nautilus is not a place for storing large-scale data. Please backup your data to your local storage and [purge](https://ucsd-prp.gitlab.io/userdocs/storage/purging/) unused data on Nautilus after finishing your project.**


## Useful resources
- Portal: https://portal.nrp-nautilus.io/ 
- Kubernetes tutorial: https://learnk8s.io/blog/kubectl-productivity/
- Kubernetes documentation: https://kubernetes.io/docs/home/
- Grafana monitoring: https://grafana.nrp-nautilus.io/dashboards/f/fef6b8ca-9d75-439c-8591-6825b1c507d1/default
- Docker hub: https://hub.docker.com/

