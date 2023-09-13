# AWS-EKS: Getting started
Research and apply concepts and tools to create a Kubernetes cluster on AWS Elastic Kubernetes Service (EKS).

## Motivation
At the end of this set of instructions, you will have a running Amazon EKS cluster that you can deploy applications to.

## Method and results

First, introduce and motivate my chosen methods, and explain how they contributes to solving the research question/business problem in the above description.

Using the AWS CLI (from now on aws cli) is the command line tool for interacting with AWS services. We need to install it and configure it for SSO login or short-tem credentials usage when it comes to authentication workflows.

kubectl – Install this command line tool for working with Kubernetes clusters. There are versions for Linux, Windows and MacOS.
eksctl – Install this command line tool for working with EKS clusters that automates many individual tasks. Is available in different operating systems also.

At the end you will have a first cluster created. We are ready now to create our first application and deploy it to EKS.

## Prerequisites

A step by step series will show you you how to get all the tools installed and IAM permissions setup for your environment.
Before starting this tutorial, you must install and configure the following tools and resources that you need to create and manage an Amazon EKS cluster:

AWS CLI - you must authenticate using [IAM Identity Center with automatic token refresh] (https://docs.aws.amazon.com/sdkref/latest/guide/access-sso.html)
 or [Authenticate with short-term credentials] (https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-short-term.html). I prefer the first one and the reason is to comply with SSO standards and authentication workflows via federation identity providers like Active Directory or Okta.
 
kubectl – A command line tool for working with Kubernetes clusters. For more information, see.
[Installing or updating kubectl] (https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html).
eksctl – A command line tool for working with EKS clusters that automates many individual tasks. For more information, see [Installing or updating eksctl] (https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html).

Required IAM permissions – The IAM security principal that you're using must have permissions to work with Amazon EKS IAM roles, service linked roles, AWS CloudFormation, a VPC, and related resources. For more information, see [Actions, resources, and condition keys for Amazon Elastic Container Service for Kubernetes](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelastickubernetesservice.html) and [Using service-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the IAM User Guide. You must complete all steps in this guide as the same user. 

In my case I created an admin user with *PowerUserAccess* permission set, so that user can do everything on AWS services except managing Users and Groups.
To check the current user, run the following command:
```
aws sts get-caller-identity
```

## Create your cluster with eksctl
Following are the steps I took to deploy my cluster on [AWS Fargate](https://docs.aws.amazon.com/eks/latest/userguide/fargate.html), which is is a serverless compute engine that lets you deploy Kubernetes Pods without managing Amazon EC2 instances.
For more information, see [Create your first cluster -eksctl] (https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

### Step 1: Create your AWS EKS control plane and nodes.
```
eksctl create cluster --name my-cluster --region region-code --fargate

# In my case the region was "us-east-1" and the cluster name is the same as shown above
```
Cluster creation takes several minutes. During creation you'll see several lines of output. The last line of output is similar to the following example line.
*EKS cluster "my-cluster" in "region-code" region is ready*
  
After cluster creation is complete, view the AWS CloudFormation stack named eksctl-my-cluster-cluster in the AWS CloudFormation console at (https://console.aws.amazon.com/cloudformation) to see all of the resources that were created.

### Step 2: View Kubernetes resources
1. View your cluster nodes, also called minions.
```
kubectl get nodes -o wide
```
2. View your workloads running on your cluster nodes
```
kubectl get pods -A -o wide
```
### Step 3. Delete your cluster and nodes
After you've finished with the cluster and nodes that you created for this tutorial, you should clean up by deleting the cluster and nodes with the following command. If you want to do more with this cluster before you clean up, see Next steps.
```
eksctl delete cluster --name my-cluster --region region-code
```
## Conclusions:
As of 09/12/2023: eksctlk and kubectl are two great command line tools that provide you seemlesly integration and great performance when it comes to creating a Kubernetes cluster on AWS-EKS.

## Next Steps:
[See here] (https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html#gs-eksctl-next-steps)

## Authors

* **Alexander Martinez Fajardo** - *Initial work* - [Alex00Pep](https://github.com/alex00pep)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc

