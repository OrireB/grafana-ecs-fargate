# 📘 Detailed Deployment Guide: Grafana on AWS ECS Fargate

This guide provides a step-by-step walkthrough for deploying Grafana using **Amazon ECS with AWS Fargate**, including setup, networking, and testing.

---

## 🧭 Objective

Deploy a Grafana containerized application that runs entirely on **serverless compute (Fargate)**, automatically receives a public IP, and is accessible on port `3000`.

---

## Prerequisites

- AWS account with sufficient permissions for ECS, VPC, and Security Groups.  
- Basic knowledge of containers and AWS networking concepts.  
- Docker Hub access (Grafana official image: `grafana/grafana`).  

---

## ⚙️ Steps to Deploy Grafana on ECS Fargate

### **Step 1 – Create an ECS Cluster**
1. Go to the **AWS Management Console** and navigate to **ECS**.
2. Open the **ECS Console** → **Create Cluster**
3. Choose **“Fargate only(Powered by Fargate)”**  
4. Name it `obs-grafana-cluster`  
5. Click **Create**

![ECS Cluster](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/48d95847d4f7fa29e16c5306ea6ba3435e49d299/docs/screenshots/ECS%20Cluster.png)
*Figure 1: ECS Cluster created using Fargate.*

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

![Task Definition](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/48d95847d4f7fa29e16c5306ea6ba3435e49d299/docs/screenshots/Task%20Definition.png)  
*Figure 2: ECS Task Definition configured with Grafana container.*

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

![Security Group](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/48d95847d4f7fa29e16c5306ea6ba3435e49d299/docs/screenshots/Security%20Groups.png)
*Figure 3 Security Group*

---

### **Step 4 – Create a Service**
1. Go to your **ECS → Cluster** → click **obs-grafana-cluster**
2. Click “Deploy → **Create Service**"
3. Task Definition: `grafana-task`
4. Service name: `obs-grafana-service`  
5. Launch type: **Fargate** 
6. Number of tasks: `1`  
7. Under **Networking**
   - Choose a **VPC** (default is fine)
   - **Subnets:** select public subnets
   - Attach the grafana-sg security group you created
   - Enable Auto-assign public IP: **ENABLED**
8. Click Create Service.
*AWS will start your Grafana container.*

![ECS Service](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/e2b23bb244213a734f275adcd378d890a440a279/docs/screenshots/Service%20Creation.png)  
*Figure 4: ECS Service running Grafana task.*

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
http://3.87.42.85:3000
*You should see the Grafana login page 🎉*
6. Log in with default credentials:  
- **Username:** `admin`  
- **Password:** `admin`

🎉 You should now see the **Grafana login page** — the fashion boutique for data is officially open!

![Grafana Login](https://raw.githubusercontent.com/OrireB/grafana-ecs-fargate/48d95847d4f7fa29e16c5306ea6ba3435e49d299/docs/screenshots/Grafana%20Login.png)  
*Figure 5: Grafana login page accessed via public IP.*

---

## Screenshots Summary

All screenshots referenced above are stored in `docs/screenshots/`:

- ECS Cluster creation  
- Task Definition setup  
- ECS Service running Grafana  
- Security Group allowing port 3000  
- Grafana login page  

---

## Notes

- After the first login, Grafana will prompt you to change the default password.  
- Ensure your public IP or DNS remains consistent if you plan to access Grafana frequently.  
- This setup demonstrates a fully serverless deployment using Fargate — no EC2 instances are required.


---

## 🧠 Technical Notes

- **Fargate Launch Type:** Serverless, eliminates EC2 management.
- **grafana/grafana Image:** Official Docker Hub image maintained by Grafana Labs.
- **Public Subnet:** Needed for direct internet access.
- **Security Group:** Protects traffic; only port 3000 is exposed.
- **ECS Service:** Ensures Grafana task auto-recovers if it stops.

---

## 🧩 Troubleshooting

| Issue | Cause | Fix |
|--------|--------|-----|
| Grafana not reachable | No public IP or incorrect security group | Enable public IP and open TCP 3000 |
| Task failed to start | Insufficient memory | Increase memory to at least 512 MiB |
| Port conflict | Port not mapped | Add port mapping 3000:3000 in task definition |

---

## 🧹 Cleanup

1. Delete **ECS Service**  
2. Delete **ECS Cluster**  
3. Deregister **Task Definition**  
4. Remove **Security Group** and related VPC resources  

---

## 🧠 Lessons Learned

- Fargate simplifies container deployment and scaling.  
- ECS Services handle automatic task recovery.  
- Understanding VPC and Security Groups is essential for cloud security.  
- Grafana can be easily containerized for modern monitoring workflows.

---

## 📈 Future Improvements

- Automate deployment with **Terraform or AWS CDK**  
- Add persistent storage (EFS) for Grafana data  
- Integrate with **Prometheus** or **CloudWatch** as data sources  
- Configure alerts and email notifications within Grafana  

---

> 🏁 **Result:** A fully functional Grafana dashboard running on a serverless, containerized architecture powered by AWS ECS and Fargate.
