Workshop: Getting Started with AWS
===

In this workshop, you'll deploy a very rudimentary web application, and we'll
walk through the steps that you most probably will take yourself when building 
a real-life webapp on the AWS cloud. In the process, we'll go through some
of the core ideas when working with AWS and the cloud, as well as some of the 
core services available to you on AWS.

The services we'll use in this workshop include (but are not limited to):

- [Amazon EC2](https://aws.amazon.com/ec2)
- [Amazon VPC](https://aws.amazon.com/vpc)
- [AWS Autoscaling](https://aws.amazon.com/autoscaling/)
- [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)

See the diagram below for the completed architecture.

![Architecture](__assets/architecture.png)



## Prerequisites

### AWS account

In order to complete this workshop, you'll need an AWS account, and have login credentials
that have access to the services listed above. 

- If you're going through a classroom workshop, your facilitator may already have credentials for you to use.
  Consult with them to confirm.

- If you need an AWS account of your own, [you can create an AWS account here](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html).
  
All of the resources you will launch as part of this workshop are eligible for the AWS free tier if
your account is less than 12 months old. [More information here](https://aws.amazon.com/free).

### SSH

You will need a machine that can SSH into a remote machine.

- If you're on a **Linux** machine, you most probably already have an SSH client ready.
- If you're on a **Mac OSX** machine, you most probably already have an SSH client read.
- If you're on **Windows**:
  - On Windows 10 v1709+: you can enable the feature **OpenSSH Client (Beta)** on your machine.
  - On other versions: installing something like [Git Bash](https://git-scm.com/downloads) gives you SSH as well.

### Git

This workshop will use Git as a platform medium, so it will be handy if you already know Git.



## Modules

This workshop is broken up into multiple modules.
You must complete each module before proceeding to the next.

1. [Deploy a Network and Webapp on AWS](../../tree/lab-01)
2. [Add a Database to Your Infrastructure](../../tree/lab-02)
3. [Scale and Load Balance Your Architecture](../../tree/lab-03)

