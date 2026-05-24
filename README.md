🚀 DevOps CI/CD Pipeline with Monitoring on AWS
-------------------------------------------------------------------------------------------------------------------------------------------------
📌 Project Overview
This project demonstrates a complete end-to-end DevOps pipeline built on AWS, integrating Continuous Integration and Continuous Deployment (CI/CD) with real-time application monitoring. The application is a simple Task Manager built with Python (Flask), containerized using Docker, automatically built and deployed via Jenkins, and monitored using Prometheus and Grafana.
The project covers the full DevOps lifecycle:

Source Code Management → GitHub
Containerization → Docker
CI/CD Automation → Jenkins
Cloud Infrastructure → AWS EC2
Monitoring & Alerting → Prometheus + Grafana


🏗️ Architecture
<img width="1146" height="664" alt="image" src="https://github.com/user-attachments/assets/8abfbe7a-5806-44fb-8e6d-2e69bdd4d8c0" />

                          
🏗️ Infrastructure Setup
Service     Host                       Port   Role
Jenkins     AWS EC2                    8080   Manages the full CI/CD pipeline
Flask App   Docker container on EC2    5000   Runs the Task Manager app
Prometheus  AWS EC2                    9090   Scrapes /metrics from Flask
Grafana     AWS EC2                    3000   Visualises Prometheus data

🛠️ Tech Stack
CategoryTechnologyVersionApplicationPython + Flask3.9ContainerizationDockerlatestCI/CDJenkinslatestCloudAWS EC2 (Ubuntu)22.04MonitoringPrometheuslatestDashboardGrafanalatestVersion ControlGit + GitHub—

📂 Project Structure
DevOps_CI-CD_Pipeline_with_Monitoring_on_AWS/
│
├── app.py                      # Flask app + Prometheus metrics
├── Dockerfile                  # Container build instructions
├── requirements.txt            # flask, prometheus_client
├── templates/
│   └── index.html              # Task Manager UI
├── project explaination.pdf    # Detailed project documentation
└── README.md

🐍 Application — Flask Task Manager
The core app app.py is a Task Manager with 4 routes:
RouteMethodWhat it does/GETShow all tasks/addPOSTAdd a task (saved as Pending + timestamp)/complete/<id>GETMark task as Completed/delete/<id>GETRemove task from list
Prometheus Metrics — exposed at /metrics
MetricTypeDescriptiontasks_created_totalCounterTotal tasks ever addedtasks_completed_totalCounterTotal tasks marked completedtasks_pending_totalGaugeLive count of pending tasks

🐳 Dockerization
dockerfileFROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
Build and run locally:
bashdocker build -t devops-demo-app .
docker run -p 5000:5000 devops-demo-app


⚙️ CI/CD Pipeline — Jenkins
Jenkins automates the entire build and deployment workflow. Every push to the GitHub repository triggers the pipeline.
Pipeline Stages
<img width="915" height="177" alt="image" src="https://github.com/user-attachments/assets/f8898e29-86cd-40ce-88b2-a558d53580b8" />

Jenkins Setup on AWS EC2

Launch an EC2 instance (Ubuntu 22.04, t2.medium recommended)
Install Java, Jenkins, and Docker:

bash   sudo apt update
   sudo apt install -y openjdk-17-jdk
   # Install Jenkins (official repo)
   sudo apt install -y jenkins
   # Install Docker
   sudo apt install -y docker.io
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins

Access Jenkins at http://<EC2-Public-IP>:8080
Create a new Pipeline job and configure GitHub repo URL
Add a webhook in GitHub → http://<EC2-Public-IP>:8080/github-webhook/


📊 Monitoring — Prometheus + Grafana
<img width="1127" height="273" alt="image" src="https://github.com/user-attachments/assets/76c712e9-ae90-49be-83b8-a200183c839f" />


Prometheus Configuration
Prometheus is configured to scrape the Flask app's /metrics endpoint every 15 seconds.
prometheus.yml:
yamlglobal:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'flask-app'
    static_configs:
      - targets: ['localhost:5000']
Run Prometheus:
bash./prometheus --config.file=prometheus.yml
Access at: http://<EC2-Public-IP>:9090
Grafana Dashboard
Grafana connects to Prometheus as a data source and visualizes the custom metrics.
Setup Steps:

Install and start Grafana on EC2
Access at http://<EC2-Public-IP>:3000 (default login: admin/admin)
Add Prometheus as a data source: http://localhost:9090
Create a new dashboard and add panels using PromQL queries:

PanelPromQL QueryTotal Tasks Createdtasks_created_totalTotal Tasks Completedtasks_completed_totalCurrent Pending Taskstasks_pending_totalTask Completion Raterate(tasks_completed_total[5m])

🚀 How to Run the Project
Prerequisites

AWS EC2 instance (Ubuntu 22.04)
Docker installed
Jenkins installed and running
Prometheus and Grafana installed
Security Group rules: open ports 5000, 8080, 9090, 3000

Step-by-Step
1. Clone the repository:
bashgit clone https://github.com/Vaish404/DevOps_CI-CD_Pipeline_with_Monitoring_on_AWS.git
cd DevOps_CI-CD_Pipeline_with_Monitoring_on_AWS
2. Run the app manually (without CI/CD):
bashpip install -r requirements.txt
python app.py
3. Run with Docker:
bashdocker build -t devops-demo-app .
docker run -d -p 5000:5000 devops-demo-app
4. Trigger Jenkins pipeline:

Push any change to GitHub → Jenkins auto-triggers → builds and deploys Docker container

5. View metrics:

App: http://<EC2-IP>:5000
Raw metrics: http://<EC2-IP>:5000/metrics
Prometheus: http://<EC2-IP>:9090
Grafana: http://<EC2-IP>:3000


🔒 AWS Security Group Ports
PortService22SSH5000Flask Application8080Jenkins9090Prometheus3000Grafana

📈 Key Learnings & Skills Demonstrated

Setting up a fully automated CI/CD pipeline from scratch on cloud infrastructure
Containerizing a Python web application using Docker
Integrating GitHub webhooks with Jenkins for automated deployments
Instrumenting a Flask application with custom Prometheus metrics
Building Grafana dashboards for real-time operational visibility
Managing AWS EC2 instances and security group configurations



# DevOps_CI-CD_Pipeline_with_Monitoring_on_AWS
[![Build Status](http://35.174.116.177:8080/job/devops-demo-pipeline/badge/icon)](http://35.174.116.177:8080/job/devops-demo-pipeline/)
