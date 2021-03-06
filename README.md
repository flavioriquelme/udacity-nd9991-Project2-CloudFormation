# udacity-nd9991-Project2-CloudFormation
Delivery of "Deploy a high-availability web app using CloudFormation" Project 2

## Project Diagram
![image](https://user-images.githubusercontent.com/34523962/126241853-84fc2da1-1cd4-4b15-8b38-39fa9c8503f8.png)

## Steps
#### 1) Configure your aws cli credentials using this command on terminal

`aws configure`

#### 2) Create S3 bucket and upload code to it 
This is the udagram S3 bucket name where developers will deliver their code to be automatically deployed into app servers. 

`cd ./code`
`aws s3api create-bucket --bucket udagram-code --region us-east-1`
`aws s3api put-object --bucket udagram-code --key index.html --body index.html`

#### 3) Create a parameter to store S3 bucket name on AWS Systems Manager Parameter Store. 
I'm using the Parameters Store service just to showcase how it works, for learning reasons.

`aws ssm put-parameter --name UdagramS3bucket --type String --value "udagram-code"`

#### 4) Create the infrastructure stack using this command:

`aws cloudformation create-stack --stack-name project2infra --template-body file://project2infra.yml --parameters file://project2infra-params.json --region us-east-1`

#### 5) Create the servers and S3 stack using this command: 

`aws cloudformation create-stack --stack-name project2servers --template-body file://project2servers.yml --parameters file://project2servers-params.json --region us-east-1 --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"`

#### 6) Test it: Reach the Load Balancer URL exported by output key LBPublicUrl

#### 7) To delete the servers stack:

`aws cloudformation delete-stack --stack-name project2servers`

(wait for its completion to proceed to next step)
Notice: The S3 bucket deletion policy is set to Retain in the cloudformation scripts. Therefore, you need to delete it manually using these commands:

`aws s3api delete-object --bucket udagram-code --key index.html`
`aws s3api delete-bucket --bucket udagram-code`

#### 8) To delete the infrastructure stack:

`aws cloudformation delete-stack --stack-name project2infra`
