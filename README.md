# Kubernetes-Deployment-EKS-ECR-DOCKER-GIT
clone a backend application from GitHub. Build a Docker image of the backend. Push my image to an Amazon ECR repository. Troubleshoot installation and configuration errors.

# Set Up Kubernetes Deployment


**Author:** Darryl Brown  
**Email:** darrylbrown1991@gmail.com

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

In this project, I will clone a backend application from GitHub. Build a Docker image of the backend. Push my image to an Amazon ECR repository. Troubleshoot installation and configuration errors.

### Tools and concepts

I used Amazon EKS, Git, Docker, and Amazon ECR to prepare a backend app for deployment with Kubernetes. This involved setting up an EKS Cluster, cloning code from Github, building and pushing a Docker image to Amazon ECR. 

### Project reflection

This project took me approximately 30-40 minutes. The most challenging part was running into the multiple error, but I wouldn't call it challenging because the error message said what was need and was an easy fix, which was all my favorite, solving the errors.

Something new that I learnt from this experience was the VPCs and Networking securities needed for Kubernetes to continuously run successful. 

---

## What I'm deploying

To set up today's project, I launched a Kubernetes cluster. Steps I took to do this included launching EC2, installing eksctl, modifying IAM role for my instance so that it has administrator access, I also ran a eksctl command.

### I'm deploying an app's backend

Next, I retrieved the backend that I plan to deploy. An app's backend means the backend is the "brain" of an application. It's where the app processes user requests and stores and retrieves data. Unlike frontend code, which is what users see and interact with, backend code works on the server side (i.e. in the background) to make sure the app behaves as expected like loading a new page when a user does things like clicking on buttons. I retrieved backend code by cloning the code from a Github repository a developer team member made.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step is to build a container image of the backend. This is because Kubernetes needs a container image for a successful app deployment. App the moment I haven't prepared a container image just raw code. 

When I tried to build a Docker image of the backend, I ran into a permissions error because the commands I ran to install Docker installed it for the root account, so only my root user can talk to the Docker engine. But, I've actually set up this Instance Connect session with another user called ec2-user, so Docker commands are blocked. The Docker commands I ran before this worked because they're prefixed with sudo, which lets a non-root user run commands with root user rights. But, it's good practice to give my ec2-user the permission instead of using sudo each time.

To solve the permissions error, I  added ec2-user to the Docker group. The Docker group is a group in Linux systems that gives users the permission to run Docker commands. By default, only the root user can run Docker commands. When I add a user (ec2-user) to the Docker group, it lets me run Docker commands without typing sudo every time.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I'm using Amazon ECR in this project to securely store, share, and deploy container images. ECR is a good choice for the job. Because it's an AWS service, which lets Elastic Kubernetes Service (EKS) deploy my container image with minimal authentication setup.

Container registries like Amazon ECR are great for Kubernetes deployment because I get to store tagged images from a single source of truth. This means that whenever user from other services pull my container image they can do it without a manual install and can be confident they have pulled the latest version.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app's backend code, I've learnt that the app's backend fetches data based on a search topic entered.
The backend code uses Flask to connect with an external API and process the data. The backend then sends the data back as formatted JSON.

### Unpacking three key backend files

The requirements.txt file lists: 
1.) Flask==2.1.3: Flask is the web framework used to build the backend code, which I'll see in app.py.
2.) flask-restx==0.5.1: The app creates an API using an extension Flask-RESTx so that users or other applications can make requests to the backend.
3.) requests==2.28.1: The Requests library is used to get data from the Hacker News API.
4.) werkzeug==2.1.2: Werkzeug helps Flask handle application-level routing. For example, when a user/service makes a request to the backend, Flask uses its routing system to direct that request to the right function in app.py, which then processes the request and responds to it.

The Dockerfile gives Docker instructions on defines how a Docker image of the backend should be built. It installs the necessary dependencies, copies the application code, and sets up the commands a container needs to run to start the Flask app. Key commands in this Dockerfile include:
1.) FROM python:3.9-alpine
2.) LABEL Author="NextWork" 
3.) WORKDIR /app
4.) COPY requirements.txt requirements.txt 
5.) RUN pip3 install -r requirements.txt
6.) COPY . . 
7.) CMD ["python3", "app.py"] 

The app.py file is the main code for your backend. It has three main parts: 
1.) Setting up the app and routing
2.) Fetching data
3.) Sending the response

---

---
