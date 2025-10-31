# ☁ Task 8: Deploy a Dockerized Web Application on AWS

## 🎯 Objective
To understand **containerization** and **cloud-native deployment** by creating a **Docker image** of a simple web application and deploying it on **AWS Elastic Container Service (ECS)** using **Fargate**.

This project demonstrates how modern cloud applications are made **portable**, **scalable**, and **easy to deploy** using Docker and AWS.

---

## 🧰 Tools & Technologies Used
- **Programming Language:** Python (Flask Framework)
- **Containerization:** Docker
- **Cloud Platform:** AWS ECS (Fargate)
- **Registry:** Amazon Elastic Container Registry (ECR)
- **Editor:** Visual Studio Code
- **CLI Tools:** AWS CLI, Docker CLI

---

## 🧩 Project Structure

task8/
│
├── app.py
├── requirements.txt
├── Dockerfile
└── README.md



---

 Application Code (Flask App)
python

from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Karishma's Dockerized Cloud App deployed on AWS ECS!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080```

    
-----
requirements.txt

flask
-----


Dockerfile:

Use a lightweight base image
FROM python:3.9-slim
Set working directory
WORKDIR /app
Copy all project files
COPY . .
Install dependencies
RUN pip install -r requirements.txt
Expose the port Flask runs on
EXPOSE 8080
Command to start the app
CMD ["python", "app.py"]

---
Steps to Build & Deploy on AWS ECS

Step 1️⃣: Build Docker Image Locally
docker build -t mycloudapp .
docker run -p 8080:8080 mycloudapp

Step 2️⃣: Push Image to Amazon ECR
Create ECR Repository
aws ecr create-repository --repository-name mycloudapp --region us-east-1
Authenticate Docker with ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
Tag and Push the image
docker tag mycloudapp:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/mycloudapp:latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/mycloudapp:latest

Step 3️⃣: Deploy on AWS ECS (Fargate)
Go to ECS Console → Create Cluster → Select Fargate.

Create a Task Definition using the pushed ECR image.

Configure Service → Launch Type: Fargate → Public Load Balancer.

Deploy and wait for the Running Status: ACTIVE.

Copy the Public Load Balancer URL.

✅ Visit your app in browser — it should display our Flask message!
# Hello from Karishma's Dockerized Cloud App deployed on AWS ECS! 

---
SHORT NOTE:

This container runs a Python Flask web app that responds with a “Hello” message.
It’s packaged using Docker, stored in AWS ECR, and deployed on AWS ECS (Fargate) for serverless scalability and easy cloud hosting.
