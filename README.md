# sagemaker-dvc-demo

Machine Learning Development includes dealing with data, code and models and we need to use a mechanism to track changes in each of these. 

This repository is for active development of a demo that can demosntrate how to use AWS Sagemaker Service for the purpose of ML and also track the the changes in the artifacts (Code, data and models).

For the purpose of this demo, we use:

- Git to track changes in code
- Data Version Control (DVC) to track changes in data
- Amazon S3 to store ML training data
- SageMaker Experiment to track the ML model and experiment observations


## Prerequisites
- AWS Account
- Git
- DVC

## Initial Setup

Note: The initial setup need to be done just for the first time

### Step 1: Create a Repository (e.g. Github) for source code


### Step 2: [Setup Sagemaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html)

Note: Currently Sageaker studio is not available in all regions. This demo was built in North Virginia Region

Note: make sure to attach S3 Full acess policy to sagemaker execution role

### Step 3: [Create an S3 Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html)

Note: Make sure to create this bucket in the same region as SageMaker studio
- This bucket will be used to store ML training data and DVC artifacts

### Step 4: Setup git in sagemaker studio

- Open SageMaker Studio and from Git menu bar, choose “open git repo in terminal”

- Use the terminal and the following commands to setup git in SageMaker studio
  
        mkdir sagemaker-dvc-demo
        cd sagemaker-dvc-demo
        git config --global user.name "USERNAME" #Replace USERNAME with your github username
        git config --global user.email "EMAIL" #Replace with your git account email address
        git init
        git remote add origin <remote> #Replace REMOTE with your git repo url

- (Optional) To avoid entering password every time with git, generate a ssh key, add it to your git account and update the remote origin url
  
       ssh-keygen -t rsa -b 4096 -C "EMAIL"  ##Replace with your git account email address
       cat  ~/.ssh/id_rsa.pub   # copy the content and add it to git account
        git remote set-url origin git+ssh://git@github.com/USERNAME/REPONAME.git  # Replace USERNAME and REPONAME with yours

- Pull the repo

        git pull origin master

### Step 5: Install DVC & commit changes to Git

        pip install dvc
        dvc init
        git commit -m "initial commit"

Note: DVC automatically add the changes to git commit

### Step 6: Setup the remote for DVC (we use S3 as the remote) and commit the changes to git

    dvc remote add -d storage s3://BUCKETNAME/  # use the name of bucket that was created in step 3
    git commit .dvc/config -m "initialize DVC local remote"
    dvc remote add  s3cache s3://BUCKETNAME/cache # use the name of bucket that was created in step 3
    dvc config cache.s3 s3cache
    dvc add --external s3://BUCKETNAME/data # use the name of bucket that was created in step 3
    git add .dvc/config data.dvc
    git commit -m "add source data to DVC"
    git push --set-upstream origin master

### Step 7: Create a new notebook (use Python3 (Data Science) kernel) in SageMaker studio and use main.ipynb as a refrence to create the experiments in SageMaker











