# Deploy sample react application using AWS Code pipeline:

1. Create IAM Role for EC2 & AWS Code Deploy: 

Step 1) Create IAM Role for EC2 and AWS Code Deploy 

Create a new role for EC2 and attach AmazonS3ReadOnlyAccess policy which will allow our EC2 instance to access stored artifacts from the Amazon S3 bucket. 

![Screenshot (98)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/c1ab0837-a9e0-4526-97a1-9499cd03f6ab)

![Screenshot (99)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/9fcff88e-98d3-419a-954e-ea6ad1685ac1)

Create a new service role for CodeDeploy and attach AWSCodeDeployRole policy which will provide the permissions for our service role to read tags of our EC2 instance, publish information to Amazon SNS topics and much more task. 

![Screenshot (100)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/511eed8f-bd5a-4792-9061-29a80c8dfb3e)

![Screenshot (101)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/350cc25b-4427-4213-a140-d2a103d98628)

Step 2) Launch a Linux EC2 instance. 

 Now, Let's Attach that EC2 Role with it. 


 ![Screenshot (102)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/e52b5b3c-eae0-4a82-83f2-ab50b2472a79)

![Screenshot (103)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/b9f287ee-3df6-4ee7-b3b0-45bb3694041e)

In security groups added inbound rule ->custom TCP -> 3009 -> AnymoreIPv4 -> saved 

![Screenshot (104)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/02edc29f-a5a9-4d0e-85ff-edab5245b4cf)

EC2 instance connected and then Install code deploy agent by following commands on terminal. 

For Centos & Amazon Linux 2 

Move to the root user: 

Sudo su 

cd 

sudo yum update 
sudo yum install ruby 
sudo yum install wget 
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install 
chmod +x ./install 
sudo ./install auto 
sudo service codedeploy-agent status 

 

Create a new server folder where our codes will be stored: 

 -> come out of the root user 

   exit 

  mkdir server 

2. Push all your codes in Github repo. 

Step 3) Create a CodePipeline using Github, CodeBuild and CodeDeploy 

a) Create CodePipeline 

Let’s navigate to CodePipeline via AWS Management Console and click on Create pipeline: 

b) Choose Github in Code Source 

After selecting GitHub as the source provider, click on the Connect to GitHub button. You’ll then be prompt to enter your GitHub login credentials 

Once you grant AWS CodePipeline access to your GitHub repository, you can select a repository and branch for CodePipeline to upload commits to this repository to your pipeline 

c) Configure CodeBuild . 

If you haven’t created a project prior to creating your pipeline, then you can create a project directly from here by clicking Create project button. 


![Screenshot (105)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/44ab1c6e-756d-4935-b8bb-364a3b2f39ec)

![Screenshot (106)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/34a29ed2-a4ad-418d-9ab6-f06fd85241ab)

![Screenshot (107)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/6890300e-139f-45de-9d21-cd842222bb5a)

![Screenshot (108)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/77bb6e59-405a-4eb4-aa08-0191a9a411f4)

![Screenshot (109)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/f47c4efc-7f66-4eaf-9341-3cdc99b86b43)

![Screenshot (110)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/924725ba-c9c8-491e-8108-3dff622dcdf5)

Note: Buildspec file is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build. For my project, I created a buildspec.yaml file and added it in the root of my project directory: 

  

d) Add Depoly Stage 

Note : Before going to configure Add Depoly Stage, Let's make duplicate tab of current tab. 

and then go to code deploy in the nevigation, Select Application, then add create a deployment group. 

 

In deployment group Select EC2 instances and select Tag and Value 

Untick Load Blancer Option 

Finally Come on Add Deploy Stage and select that created Application name & Deployment group 

![Screenshot (111)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/7e9a53f2-d608-4b4a-b3a3-b49e676b977d)

![Screenshot (112)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/983adb48-7633-4285-88a1-ffbd5f81122a)

![Screenshot (113)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/d2002f54-180e-4fab-b056-b353c0c80392)

![Screenshot (114)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/03445439-1254-4975-ae78-5ddf31db4791)

Just go to pipeline and add configure the pipeline with the code build and deploy 


 ![Screenshot (115)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/bdba3e95-b6b7-4e4c-aca5-dd1bd97d67e5)

![Screenshot (116)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/173f9625-28dc-46a3-90d7-ef836354a502)

e) At the end, just review and create 

 using public IP address -> add port 3009 -> see the application. 

![Screenshot (117)](https://github.com/HIMA10SHREE/AWS_CI-CD/assets/52618743/583a9859-7c3d-4cfd-b248-d1694ce9fc61)

Note: if your code deploy is giving script timeout in afterinstall script: 

Increase the size of instance follow one of the steps: 

1)T2.small or t2.medium 

2)increase the script time in the script. 

 
If You will Get below error in pipeline, then follow further instruction to resolve it. 
 
 

Error: CodeDeploy agent was not able to receive the lifecycle event. Check the CodeDeploy agent logs on to your host and make sure the agent is running and can connect to the CodeDeploy server. 
 

Conclusion: Its happening Because we installed codedeployment-agent before attaching ec2role to EC2 Instance, and error will be resolve when you will first attach ec2role and then install codedeployment-agent on EC2. 

 
