# sagemaker-dvc-demo

Machine Learning (ML) applications can change in three axes (data, code and model) and we need to implement a mechanism to track the changes during the development and delivery process. This repository is for active development of a demo that can demosntrate how to use AWS Sagemaker Service for the purpose of ML application development and also track the changes in code, data and models.

For the purpose of this demo, we use:

- Git to track changes in code
- Data Version Control (DVC) to track changes in data stored in Amazon S3
- Amazon SageMaker Experiment to track the experiment observations


## Prerequisites
- AWS Account
- Git
- DVC

## Initial Setup

Note: This initial setup needs to be done just for the first time

### Step 1: 

Create a Repository (e.g. [Github](https://docs.github.com/en/enterprise/2.16/user/github/getting-started-with-github/create-a-repo)) for source code (e.g. sagemaker-dvc-demo)


### Step 2: 

[Setup Sagemaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html)

Note: Currently Sageaker studio is not available in all regions. This demo was built in North Virginia Region

Note: make sure to attach S3 Full acess policy to sagemaker execution role

### Step 3: 

[Create an S3 Bucket to store data and DVC artifacts](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html)


Note: Make sure to create this bucket in the same region as SageMaker studio

### Step 4: 

Create a folder named data in S3 bucket

### Step 5: 

Setup git...

- Open SageMaker Studio and from Git menu bar, choose “open git repo in terminal”

- Use the following commands and setup git in SageMaker studio
  
        mkdir sagemaker-dvc-demo
        cd sagemaker-dvc-demo
        git config --global user.name "USERNAME" #Replace USERNAME with your source control username
        git config --global user.email "EMAIL" #Replace with your source control accouunt email address
        git init
        git remote add origin <remote> #Replace REMOTE with your git repo. url

- (Optional) To avoid entering password every time with git, you can generate a ssh key, add it to your git account and update the remote origin url
  
       ssh-keygen -t rsa -b 4096 -C "EMAIL"  ##Replace with your source control account email address
       cat  ~/.ssh/id_rsa.pub   # Copy the content and add it to git account
        git remote set-url origin git+ssh://git@github.com/USERNAME/REPONAME.git  # Replace USERNAME and REPONAME with yours

- Pull the repo

        git pull origin master

### Step 6: 

Install DVC & commit changes to Git

        pip install dvc
        dvc init
        git commit -m "initial commit"

Note: DVC automatically add the changes to git commit

### Step 7: 

Setup the remote for DVC (we use S3 as the remote for DVC) and commit the changes into git

    dvc remote add -d storage s3://BUCKETNAME/  # use the bucket name that was created in step 3
    git commit .dvc/config -m "initialize DVC local remote"
    dvc remote add  s3cache s3://BUCKETNAME/cache # use the bucket name that was created in step 3
    dvc config cache.s3 s3cache
    dvc add --external s3://BUCKETNAME/data # use the bucket name that was created in step 3
    git add .dvc/config data.dvc
    git commit -m "add source data to DVC"
    git push --set-upstream origin master

### Step 8: 

Create a new notebook (use Python3 (Data Science) kernel) in SageMaker studio and follow main.ipynb notebook as a refrence point to create the experiments in SageMaker and track the changes.











