
# CICD Project without ArgoCD ( Push Based Deployment )

This project is all about CICD pipeline without ArgoCD . We are going to create CICD project by simply hosting spring boot application on AWS EKS cluster . we are using push based deployment approach here . 

## There are two types of deployment :-

-  Push Based Deployment : Changes are directly pushed on K8S cluster 

-  Pull Based Deployment : An agent is installed on K8S cluster , that agent will monitor infra git repository & as soon as there is a new commit in infra git repo , this agent will syncs all new changes to its current environment . it compares desired state & actual state .

## Git Repository For application code available at below link:

https://github.com/akashzakde/Java_CICD_Project_Kubectl.git

## Tools and technology used in our project are : 

-  Jenkins
-  Git
-  Maven
-  SonarQube Scanner & Server
-  Docker & Docker Hub Registry
-  AWS EKS Cluster
  
## Below is our CICD Project Diagram :

<img width="643" alt="CICD Project With EKS Push Based Deployment" src="https://github.com/akashzakde/Java_CICD_Project_EKS/assets/64258131/6b39e118-ac4a-488f-a846-5e3312a4eecb">


## Installation Steps :

Step 1 : Login to AWS management console using our email id & password

Step 2 : Create Security Group For jenkins & sonarqube server , Allow port 22 , 8080 in jenkins security group & allow 22 , 9000 in sonarqube security group .

Step 3 : Create Security Key Pair .

Step 4 : Launch two t2.medium EC2 instances (one for jenkins & another for sonarqube server).

Step 5 : Login to jenkins server using public ip address & setup jenkins server .

Step 6 : Login to sonarqube server using public ip address & setup sonarqube server .

Step 7 : Now create AWS EKS cluster , you can create EKS cluster using terraform by using this github project https://github.com/akashzakde/eks-cluster-using-terraform.git

Step 8 : Login to jenkins dashbaord using url http://{jenkins-server-ip}:8080 

Step 9 : Install Maven Integration, Github Integration, Sonarqube Scanner plugins on jenkins .

Step 10 : We need to add sonarqube server details & its credentials , add tools like maven , git & java jdk path inside jenkins.

Step 11 : Now login to sonarqube server using url http://{sonarqube-server-ip}:9000 , create new project for java , create webhook for jenkins . 

Step 12 : We need to integrate AWS EKS cluster with our jenkins server , Inorder to integrate EKS cluster with jenkins server please follow this article : https://www.fosstechnix.com/integrate-remote-kubernetes-cluster-with-jenkins/ 

Step 13 : Login jenkins dashboard create 1 pipeline , add this repo details for CICD pipeline code i.e. https://github.com/akashzakde/Java_CICD_Project_Kubectl.git

Step 14 : Inorder to run our CICD pipeline automatically on new commit , we need to add webhook on our git repo i.e. https://github.com/akashzakde/Java_CICD_Project_Kubectl.git
