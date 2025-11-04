# grafana-ecs-fargate
# 🧵 Deploying Grafana on Amazon ECS with Fargate  
*A simple, fashion-themed walkthrough analogy for beginners*

---

## 🧭 1. Introduction
This project demonstrates how to deploy **Grafana**, an open-source data visualization and monitoring tool, on **Amazon ECS (Elastic Container Service)** using **AWS Fargate**.

Grafana helps you visualize system metrics and create beautiful dashboards — perfect for monitoring applications, servers, or business data.  
With **ECS Fargate**, you can run Grafana without managing any servers — fully serverless, scalable, and elegant.

Think of this project as setting up a chic **data boutique** in the AWS **fashion district** — where your dashboards are the stylish outfits on display.

---

## 🧩 2. Key Concepts

| Term | Meaning |
|------|----------|
| **Grafana** | A tool that lets you create dashboards from data helps you visualize and monitor data from various sources. |
| **Docker Image** | A pre-packaged app that includes everything Grafana needs to run. |
| **Container** | A running instance of that image — like a mini self-contained virtual space. |
| **Amazon ECS(Elastic Container Service)** | A service that runs and manages containers at scale. |
| **AWS Fargate** | A “serverless” compute engine that automatically runs your containers without EC2 instances. |
| **Task Definition** | A blueprint describing how your container should run (image, ports, memory, etc.). |
| **Security Group** | Acts like a firewall — controls which network traffic can reach your container. |
| **Cluster** | A logical group in ECS to organize your running containers (like a project folder). |

---

## 👗 3. The Fashion Analogy (Quick Look)

| Fashion Term | Tech Term | Description |
|---------------|------------|-------------|
| **Fashion District** | AWS Cloud | The global environment where all fashion houses (apps) live. |
| **Backstage Crew** | Fargate | Handles setup and logistics automatically — no manual work. |
| **Retail Chain** | ECS Cluster | A collection of boutiques (containers) managed under one brand. |
| **Boutique** | Container | Where your fashion (Grafana) is displayed for visitors. |
| **Lookbook** | Task Definition | Describes how each outfit (container) should look. |
| **Fashion Book** | Docker Image | The original design inspiration (Grafana’s official image). |
| **Security Guard** | Security Group | Controls who can enter your boutique (port 3000). |
| **Fashion Catalogue** | Docker Hub | Where published designs (images) are sourced. |
| **Display Window** | Grafana Dashboard | What the audience sees — your data trends in full style. |

---

## ⚙️ 4. Steps to Deploy Grafana on ECS Fargate

### **Step 1 – Create an ECS Cluster**
1. Go to the **AWS Management Console** and navigate to **ECS**.
2. Open the **ECS Console** → **Create Cluster**
3. Choose **“Fargate only(Powered by Fargate)”**  
4. Name it `obs-grafana-cluster`  
5. Click **Create**

---

### **Step 2 – Create a Task Definition**
1. Go to **Task Definitions** → **Click "Create New Task Definition"** 
2. Launch type: **Fargate**  
3. Name: `obs-grafana-task` 
4. Task size: `0.5 vCPU` and `1 GB` memory choose small values FOR TESTING
5. Add container:  
   - Container Name: `grafana`  
   - Image: `grafana/grafana`(official Grafana Docker image)
   - Port mappings:
     - Container port: `3000`
     - Protocol: `TCP`
6. Click **Add**, then **Create**
*This tells ECS: “Run one Grafana container, using the grafana/grafana image, and make port 3000 accessible.”*

---

### **Step 3 – Create a Security Group (for port 3000)**
1. Go to the VPC dashboard and click **"Security Groups."**
2. Create a new security group called: obs-grafana-sg
   - Add an inbound rule allowing traffic on port 3000.
   - Add an Inbound Rule:
	 - Type: Custom TCP
	 - Port range: 3000
	 - Source: 0.0.0.0/0 (anyone can access it — fine for testing)
3. Save.

---

### **Step 4 – Create a Service**
1. Go to your **ECS → Cluster** → click **obs-grafana-cluster**
2. Click “Deploy → **Create Service**"
3. Launch type: **Fargate**  
4. Task Definition: `grafana-task`  
5. Service name: `obs-grafana-service`  
6. Number of tasks: `1`  
7. Under **Networking**
   - Choose a **VPC** (default is fine)
   - **Subnets:** select public subnets
   - Attach the grafana-sg security group you created
   - Enable Auto-assign public IP: **ENABLED**
8. Click Create Service.
*AWS will start your Grafana container.*

---

### **Step 5 – Wait for the Service to Run**
Wait for the service to become available and the task to show status = **RUNNING**  

---

### **Step 6 – Run and Access Grafana**
1. Go to ECS → Clusters → grafana-cluster → Tasks.
2. Click your running task.
3. Scroll down to **Networking** → Find the **Public IP** and copy it.
4. Click the task → copy the **Public IP** under *Network*  
5. Visit in your browser:  http://<PUBLIC-IP>:3000
*You should see the Grafana login page 🎉*
6. Log in with default credentials:  
- **Username:** `admin`  
- **Password:** `admin`

🎉 You should now see the **Grafana login page** — the fashion boutique for data is officially open!

---

## 🎬 5. Real-World Scenario — Netflix Edition

Imagine it’s Friday night and Netflix is preparing for a major new release.  
Suddenly, their main monitoring dashboards go offline — they can’t see streaming performance, API response times, or user load.

Within minutes, engineers spin up **Grafana on ECS with Fargate**, connect it to data sources like **Prometheus** and **CloudWatch**, and restore visibility.  
Now, they can monitor streaming latency and regional uptime — all without managing a single server.

That’s the power of **serverless observability** — quick, reliable, and scalable when it matters most.

---

##💡 Real-world uses of Grafana

Grafana is used by:
	- DevOps teams to visualize server metrics (CPU, memory, server uptime)
	- Data engineers to monitor databases or APIs
	- Businesses to create dashboards showing KPIs or logs (sales, website traffic, etc.)
	- Cloud teams to connect with AWS CloudWatch, Prometheus, Loki, etc.
It’s part of the standard “observability stack” — used to keep an eye on how systems are performing.

---

## 🖼️ 6. Screenshots
- ECS Cluster and Service page
![ECS Cluster](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/a3a0ccf1c0d638d3340bcc8dbf4a8272775e727c/ECS%20Cluster.png)

---

- Task Definition showing `grafana/grafana` and port `3000`
![Task Definition](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/a3a0ccf1c0d638d3340bcc8dbf4a8272775e727c/Task%20Definition.png)

---

- Security Group inbound rule for port `3000`
![Security Group](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/a3a0ccf1c0d638d3340bcc8dbf4a8272775e727c/Security%20Group-%20Grafana.png)

---

- Grafana login page in your browser  
![Grafana Login](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/a3a0ccf1c0d638d3340bcc8dbf4a8272775e727c/Grafana%20Login.png)

---

## 🧵 7. Summary
Deploying Grafana on **Amazon ECS with Fargate** gives you:
- Serverless simplicity — no EC2 management  
- Scalable, repeatable deployments  
- A quick way to visualize metrics in real time  

💡 **In short:** Fast setup, clean dashboards, and zero server headaches — cloud monitoring, but make it fashion.

---

## 🪩 Author
Created as part of a learning mission — combining **cloud technology**, **storytelling**, and a little **runway flair** 👠  
**Orire Bankole**  
Cloud / DevOps Enthusiast 
🌐 [Twitter or X Portfolio Link](https://x.com/_Lorisann)
