# Nautilus Tutorial

## Intro

Nautilus provides computing resources (CPU/GPU) to scale up your compute. This idea is that you contribute computing resources to nautilus, the resource will be shared by contributors, then you can use all the available resources. There are thousands of GPUs, many nodes with hundreds of CPUs and nodes with a few hundred GB memories on Nautilus.

Use cases:
1. Use an 8-GPU node to make batch size larger. This will enhance model performance and shorten the training cycle.
2. Use four 8-GPU node to run ablation studies in parallel.
3. Use a node with 32 thread 300 GB memory to preprocess the data in parallel.
4. Process 1 TB data, generate 4 TB intermediate files and store the 0.5 TB final data.

As you can see, Nautilus provides many ways for you to scale up your compute and get things done faster.

Be mindful of resource.
Backup your code and data.

## Work flow

0. Preparation
    - Get access to Nautilus and familiar yourself with the restrictions.
    - Create a PVC, this is where you will store your own data and results. Large shared datasets will be downloaded in shared PVC
    - Setup a docker
1. Data transfer
    - Create a data transfer deloyment to upload data
2. Data preprocessing and initial testing
    - Data preprocessing can be run locally or on Nautilus with parallel processing.
    - Test your code with the data and single GPU
3. Scale up your compute with jobs

### Persistent Volume Claim (PVC)

There are three storage you can use, local storage, PVC and S3.

Local storage is associated with each node. They are best for small to mid size datasets with a lot of small files. Note that the data will be lost when your pod restarts. You can increase the size by increasing the empheral memory in the config.

PVC keep the data even after your pod ends. Thus they are used to store large public datasets to be shared by the lab and also store key results and checkpoints in case pods crash. Node that they are optimized for large files, and not good at handling many small files.

S3 can be a bridge to transfer data between nautilus and your computers. But you can also use it to host your dataset. They are good at handling small files. You can request an S3 account on matrix chat for Nautilus. [See further down on how to setup s5cmd]

To create a PVC, use the [pvc example](example/pvc-example.yaml). 
You will need to change the name: user-west-vol and adjust specifications if needed. PVC size can be updated on the fly without restarting the running node.

### Setup a Docker

Find a docker or create a docker image and upload to the docker hub.


### Data Transfer

Create a CPU only deployment to transfer data to Nautilus. 

### Data preprocessing

Nautilus PVC are cloud-based file system that can be paralleled. Thus you can leverage multiple threads to preprocess files and sifnificantly reduce the process time, from days to hours. This also applies to data transfer.

### Initial Test

It will be a good idea to test with a single GPU deployment to make sure the code is bug-free before scaling up. 

### Scale Up

Now once you are ready, leverage the GPU nodes to speed up your compute. Turn your deployment into a job, that is a script to run a command when the pods starts.

Check the [Nautilus resource](https://portal.nrp-nautilus.io/resources) page to see the availabilities. Select your target GPU types and filter by the number of GPUs you want (note that set the number to be x-1 in the filter as there is a bug). Sometimes you will also need to pay attention to CPU and memory constraints. Checking this website will allow you to know what you can get immediately.

We contributed a large amount of resource to Nautilus, thus we have priority to taint nodes, meaning we can reserve them for our use. When there are major conference deadlines approaching and you find it hard to secure compute, talk to our nautilus admin (Robin) to ask for tainting some nodes on Nautilus. Normally getting 4 GPU nodes are easy and you can also get 8 GPU nodes at most over night.

### Monitoring your resource

While you can attach to your node and check the resource use, using [Grafana](https://grafana.nrp-nautilus.io/d/dRG9q0Ymz/k8s-compute-resources-namespace-gpus?orgId=1&refresh=30s&var-namespace=cogrob&from=now-30m&to=now) is much easier.

1. [Namespace GPU](https://grafana.nrp-nautilus.io/d/dRG9q0Ymz/k8s-compute-resources-namespace-gpus?orgId=1&refresh=30s) Monitor the GPU utilization for pods in this namespace.
2. [Namespace CPU](https://grafana.nrp-nautilus.io/d/85a562078cdf77779eaa1add43ccec1e/kubernetes-compute-resources-namespace-pods?orgId=1&refresh=10s) Monitor the CPU and memory for pods in this namespace.
3. [Node GPU](https://grafana.nrp-nautilus.io/d/Tf9PkuSik/k8s-nvidia-gpu-node?orgId=1&refresh=15m) Check the GPU status on a specific node.

### s5cmd Setup ###
s5cmd is a useful tool for getting large amounts of data onto / off of nautilus.

1. Install s5cmd according to your system https://github.com/peak/s5cmd
2. Obtain your access keys for s3 from nautilus
3. You can get your credentials (key and secret) in the user portal, Storage->S3 Keys page for public ceph pools. To get credentials for private ones, contact the admin managing particular S3 storage. You should receive an email with your keys
4. Create a file in ~/.aws/credentials with the following contents
   
[default]

aws_access_key_id = 'your access key from step 3'

aws_secret_access_key = 'your access secret key from step 3'

aws_region = us-west-1

5. create an alias in your bashrc to make things easy
6. alias s5cmd-west='/usr/bin/s5cmd --credentials-file /home/username/.aws/credentials --endpoint-url https://s3-west.nrp-nautilus.io/'
7. source your bashrc again and test to see if things work
8. s5cmd-west ls  should produce no errors and not show anything
9. s5cmd-west mb s3://test-bucket should create a bucket which should then show up with the ls command above.
10. remove the test bucket 's5cmd-west rb s3://test-bucket'

useful link https://docs.nrp.ai/userdocs/storage/ceph-s3/

## Tips

1. Parallel data transfer and data preprocessing.
2. Use Nautilus resource page to check GPU availability and adjust your request accordingly.
3. Use Grafana to monitor the resource utilization.
4. You can update PVC size on the fly.
5. Ask admin to taint nodes for your job if needed.
6. Create a repository for all yaml and docker files for all projects. So when you start a new project you can reuse them easily.

