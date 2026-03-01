# ��� CandyCrush DevOps Pipeline

A complete end-to-end DevOps pipeline featuring CI/CD automation, security scanning, containerization, Kubernetes deployment on AWS EKS, and monitoring with Prometheus and Grafana.
<img width="1918" height="924" alt="image (14)" src="https://github.com/user-attachments/assets/0ecbe135-5ae5-4277-8d16-9f68b6b63c9d" />


## ���️ Architecture
```
GitHub Push → GitHub Actions → SonarQube → Trivy → Docker → EKS → Prometheus + Grafana
```

## ���️ Tech Stack

| Tool | Purpose |
|------|---------|
| GitHub Actions | CI/CD Pipeline |
| SonarQube | Static Code Analysis |
| Trivy | Security Scanning |
| Docker | Containerization |
| Docker Hub | Container Registry |
| Terraform | Infrastructure as Code |
| AWS EKS | Kubernetes Cluster |
| Prometheus | Metrics Collection |
| Grafana | Monitoring Dashboards |
| Slack | Notifications |

## ��� Quick Start

### 1. Install Tools on EC2
```bash
sudo apt-get update
sudo apt install docker.io -y
sudo chmod 777 /var/run/docker.sock
```

### 2. Run SonarQube
```bash
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

### 3. GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| SONAR_TOKEN | SonarQube token |
| SONAR_HOST_URL | http://<EC2-IP>:9000 |
| DOCKERHUB_USERNAME | Docker Hub username |
| DOCKERHUB_TOKEN | Docker Hub access token |
| SLACK_WEBHOOK_URL | Slack webhook URL |

### 4. Deploy EKS with Terraform
```bash
cd Eks-terraform
terraform init
terraform apply --auto-approve
aws eks --region us-east-1 update-kubeconfig --name EKS_CLOUD
```

### 5. Deploy Application
```bash
kubectl apply -f deployment-service.yml
kubectl get all
```
<img width="1467" height="697" alt="image (11)" src="https://github.com/user-attachments/assets/22c4343b-1686-4162-b772-d408382b0c7e" />


### 6. Install Monitoring
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set prometheus.service.type=LoadBalancer \
  --set grafana.service.type=LoadBalancer
```
<img width="1336" height="577" alt="image (10)" src="https://github.com/user-attachments/assets/624eeb44-56f7-43b2-bf77-c8177bb7fc5b" />
<img width="1917" height="943" alt="image (12)" src="https://github.com/user-attachments/assets/844f44d1-e928-470a-852d-5a9216669797" />



## ��� Project Structure
```
Candycrush/
├── .github/workflows/deploy.yml     # CI/CD pipeline
├── Eks-terraform/                   # Terraform EKS config
├── monitoring/                      # Prometheus + Grafana config
├── deployment-service.yml           # Kubernetes manifests
├── Dockerfile                       # Container definition
└── package.json                     # Node.js dependencies
```

## ��� Cleanup
```bash
helm uninstall prometheus -n monitoring
kubectl delete namespace monitoring
kubectl delete -f deployment-service.yml
cd Eks-terraform && terraform destroy --auto-approve
aws s3 rb s3://youngyz1-terraform-state --force
```

## ��� Author
**Youngyz1** - [@Youngyz1](https://github.com/Youngyz1)
