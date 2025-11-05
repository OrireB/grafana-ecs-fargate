# 🚀 Deploying a Containerized Grafana Dashboard using Amazon ECS and AWS Fargate

> 🧠 **Goal:** To deploy, run, and expose a containerized Grafana monitoring application on AWS using serverless infrastructure — **no EC2 instances required.**

---

## 📖 Overview

This project demonstrates how to deploy **Grafana**, a popular open-source analytics and monitoring tool, using **Amazon ECS (Elastic Container Service)** with **AWS Fargate** — achieving a **fully managed, serverless containerized environment**.

### 🎯 Objective
Deploy a Grafana containerized application that:
- Runs without manually managing servers.
- Is accessible publicly on port `3000`.
- Demonstrates networking, security, and orchestration on AWS.

---

## ⚙️ Technologies Used

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

### 🧱 AWS Services Used

| Service | Purpose |
|----------|----------|
| **Amazon ECS** | Orchestrates Docker containers and services. |
| **AWS Fargate** | Runs containers without EC2 provisioning (serverless). |
| **Amazon VPC** | Provides isolated networking environment. |
| **Security Groups** | Firewall to control inbound and outbound access. |

---

## 🏗️ Architecture

            ┌──────────────────────────────┐
            │        AWS Cloud             │
            │                              │
            │  ┌──────────────────────┐    │
            │  │   ECS Cluster        │    │
            │  │  (Fargate Launch)    │    │
            │  └───────┬──────────────┘    │
            │           │                   │
            │  ┌────────▼────────┐          │
            │  │ ECS Task (Grafana) │       │
            │  │ Image: grafana/grafana │   │
            │  │ Port: 3000             │   │
            │  └────────┬────────┘          │
            │           │                   │
            │   ┌───────▼────────┐          │
            │   │ Security Group │          │
            │   │ Allow TCP:3000 │          │
            │   └───────┬────────┘          │
            │           │                   │
            │   ┌───────▼────────┐          │
            │   │  Public Subnet │          │
            │   │  Public IP     │          │
            │   └───────┬────────┘          │
            │           │                   │
            │   Access via Browser           │
            │   http://<PUBLIC-IP>:3000      │
            └──────────────────────────────┘
---

## 📊 Skills Demonstrated

- **Container Orchestration & Deployment:**  
  Designed and launched a Grafana container through ECS, understanding how tasks and services interact in AWS.

- **Serverless Infrastructure Management:**  
  Used AWS Fargate to simplify operations — no servers, auto-scaling, and pay-per-use compute.

- **Cloud Networking & Security:**  
  Configured public subnets, assigned public IPs, and set up firewall rules that allow secure external access on port 3000.

- **Infrastructure Thinking:**  
  Adopted a declarative approach to defining infrastructure, mirroring Infrastructure-as-Code concepts.

- **Observability Mindset:**  
  Gained hands-on understanding of Grafana’s role in monitoring and metrics visualization within cloud environments.

---

## 🧩 Project Highlights

✅ **Serverless Compute:** Zero EC2 management.  
✅ **Public Access:** Exposed Grafana securely via TCP 3000.  
✅ **High Availability:** ECS service automatically restarts failed tasks.  
✅ **Scalability:** Easy to extend with multiple containers or data sources.

---

## 📸 Screenshots

Add the following to your `docs/SCREENSHOTS.md`:
- ECS Cluster & Running Service  
- Task Definition showing `grafana/grafana`  
- Security Group allowing TCP 3000  
- Grafana login page (`http://<PUBLIC-IP>:3000`)
[**docs/SCREENSHOTS.md**](docs/screenshots)

---

## 📘 Full Documentation

👉 See the full step-by-step deployment guide here:  
[**docs/DEPLOYMENT_GUIDE.md**](docs/DEPLOYMENT_GUIDE.md)

---

## 💬 Reflection

This project deepened my understanding of **container-based cloud deployment**, **serverless orchestration**, and **secure networking** in AWS.  
It’s a practical example of how DevOps teams deploy monitoring tools at scale using fully managed cloud services.

> “The best infrastructure is invisible — it just works.”

---

## 👩‍💻 Author

**Your Name**  
📧 your.email@example.com  
🌐 [GitHub](#) • [LinkedIn](#)
