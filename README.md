# sagemaker-dvc-demo

Machine Learning Development includes dealing with a lot of data, code and models and we need to use a mechanism to track each of those. 

This repository is for active development of a demo that can showcase how to use AWS Services and other open source tools to track the artifacts of ML development (Code, data and models)

For the purpose of this demo, we use:

- Git to track code changes
- Data Version Control (DVC) to track data
- SageMaker Experiment to track the models 

we use Amazon S3 as the storage for the data and DVC artifacts.
 

## Prerequisites

## Initial Setup

Note: This stage just needs to be done for the first time

### Step 1: Create an S3 Bucket (https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html)

### Step 2: Create a git repo. 

### Step 3: Setup Sagemaker Studio

Note: Currently Sageaker studio is not available in all region. You can use North Virginia Region

Note: make sure to attach S3 Full acess to sagemaker execution role

### Step 4: Create S3 bucket in the same region

### Step 5: Setup git 

- Open SageMaker Studio and from Git menu bar, choose “open git repo in termina”

- Use the terminal to setup git
  
        mkdir sagemaker-dvc-demo
        cd sagemaker-dvc-demo
        git config --global user.name "USERNAME" #Replace USERNAME with your github username
        git config --global user.email "EMAIL" #Replace with your git account email address
        git init
        git remote add origin <remote> #Replace REMOTE with your git repo url

- To avoid entering password every time, setup ssh key in your git account
  
       ssh-keygen -t rsa -b 4096 -C "EMAIL"  ##Replace with your git account email address

- Add the ssh key to your git account and update the remote
    
        git remote set-url origin git+ssh://git@github.com/USERNAME/REPONAME.git  # Replace USERNAME and REPONAME with yours

- Pull the repo

        git pull origin master

### Step 5: Setup DVC & commit changes to Git

        pip install dvc
        dvc init
        
        git commit -m "initial commit"

Note: DVC automatically add the changes to git commit

### Step 6: Setup the remote for DVC (we use S3 as the remote)

    dvc remote add -d storage s3://BUCKETNAME/

    git commit .dvc/config -m "initialize DVC local remote"


### Step 7: Add the dataset

    dvc remote add  s3cache s3://sagemaker-demo-sam/cache
    dvc config cache.s3 s3cache
    dvc add --external s3://sagemaker-demo-sam/data
    git add .dvc/config data.dvc
    git commit -m "add source data to DVC"

### Step 8: Push the changes (This push changes to both dvc and git)

    git push --set-upstream origin master

### Step 9: Open a new notebook use Python3 (Data Science) kernel

Note: Follow  main.ipynb as a refrence











