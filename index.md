

## *Welcome to my GitHub Webseit*

Since quite a while I'm interested in Cloud Engineering and especially the DevOps / Automatisation part of it. Working with AWS, Terraform, Python, Docker and Kubernetes is a thrilling adventure and there's so much to learn and work on. The purpose of this website is to show my journey going through all this stuff, working on my own projects and share my code with you.   

[My GitHub Repos](https://github.com/ThomasTusche?tab=repositories)



### *1) Project: Creating an environment with AWS, Terraform, Docker and Kubernetes for a wordpress website*

The first project I want to share with you is an entire infrastructure I created to host a wordpress website on it. 
Nearly all AWS ressources were created with Terraform and deployed via CodePipeline. The Docker container is running
inside a Kubernetes Cluster, which is deployed through a Pipeline aswell.

#### AWS
![Aws_overview](./aws_overview.png)

To create the EC2 machines, on which Kubernetes runs, I created a VPC with public and private subnets. By visiting the URL of my website, the traffic will be hitting
AWS Route53 servive which automatically redirect the traffic to the Elastic Loadbalancer. The AWS ELB is listening for Http and Https traffic and 
routes the packages to the the Kubernetes Node on which the Wordpress Service is running. This service is passing the traffic forward to the right container.

#### Terraform 
![Terraform_Pipeline](./terraform_pipeline.png)

All my Terraform Code is placed inside a AWS CodeCommit Repository. Cloudwatch is monitoring the masterbranch and runs the
AWS CodePipeline everytime there is new code pushed to master. 
My pipeline consists of four steps:
- Recognising changes in the code
- Running a "terraform plan" against the current infrastructure and display any upcoming changes
- Waiting for a manual approval or rejection
- After code approval runs the terraform apply

The Terraform code is available here:
[Terraform Repo](https://github.com/ThomasTusche/portfolio-website/tree/master/terraform)

#### Kubernetes 
![Kubernetes_Pipeline](./kubernetes_pipeline.png)

I installed a Kubernetes cluster by using kubeadm and used Calico to deploy the network for the Pods.
After the Cluster was created I stored the Yaml files for my deployments inside a CodeCommit Repo which is watched by Cloudwatch.
Any changes to master will be recognised by Cloudwatch which triggeres a CodePipeline and runs the following steps:
- Recognising the code changes
- Running a kubectl apply --dry-run
- Waiting for approval or rejection
- Running a kubectl apply and deploying the changes

The Kubernetes code is available here:
[Kubernetes Repo](https://github.com/ThomasTusche/portfolio-website/tree/master/kubernetes)

#### Docker 
![Docker_Pipeline](./docker_pipeline.png)

I created two container, one running Wordpress and the other one running the MySQL Server. Both are storing their files on the EBS
volume of the worker node, to keep the data even if pods are getting destroyed or restartet. Normally I would store the data on an EFS Share
but here I went for the most simple solution.


### *2) Project: Diving deeper into Python*

Python is incredible powerfull while being relatively easy to write. Atleast for me, it is the first language I could really stick too, being not
a developer. Here are some project and code example I did recently.

#### Blackjack
![blackjack](./blackjack.png)

The first game I tried was the casino game Blackjack. The dealer gives you card after card until you stop or reach over 21. Afterwards the "Casino"
draws it's card and, as the rules states, stops after hitting 17 or more points. The next step is to evaluate the points of the player against the
casino and decide who won. 
To make it a bit more interesting you start with a certain amount of credits and have to leave if you run out of them.

The Python code is available here:
[Blackjack Code](https://github.com/ThomasTusche/blackjack)

#### Hangman
![hangman](./hangman.png)

My second game was the Game Hangman. The projects comes with a wordlist from which a random word is being chosen. The player gets to know the amount
of letters the word has and loses the games if his guesses failes 6 times. Meanwhile each correct letter will be displayed, all guessed letters will be
displayed, that you not chose the same letter twice and a graphics shows the state of your stickfigure.

The Python code is available here:
[Hangman Code](https://github.com/ThomasTusche/hangman)

### *3) Project: AWS Cost Allocator as Shell Script inside a Docker Container*

Costs are an important factor for any infrastructure whether being hosted inside a public cloud or not. At one point I had to gather specific costs
of three different AWS Accounts and restructure the Data that could be displayed inside AWS Quicksight. Additionally, I wanted to run it inside a
docker container on a sheduled basis. The container gets triggered by a Cloudwatch Rule, downloads the configuration and scripts, excecutes them 
and upload the generated file on an S3 Bucket. 

The Shell Script and Dockerfile is available here:
[Costallocator_Projekt](https://github.com/ThomasTusche/awscli_cost_allocator)



