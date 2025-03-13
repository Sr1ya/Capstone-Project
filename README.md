# Capstone Project: ML Model Deployment with MLOps

## Overview
This project sets up an end-to-end Machine Learning (ML) pipeline with MLOps practices, integrating tools like MLflow, DVC, Docker, AWS, and Kubernetes (EKS). The pipeline enables model tracking, versioning, deployment, and monitoring using Prometheus and Grafana.

---

## 📌 Project Setup
### 1️⃣ Repository & Virtual Environment Setup
```bash
# Clone the repository
git clone <repo-url>
cd <repo-name>

# Create and activate virtual environment
conda create -n atlas python=3.10 -y
conda activate atlas

# Install cookiecutter and initialize project structure
pip install cookiecutter
cookiecutter -c v1 https://github.com/drivendata/cookiecutter-data-science

# Rename src.models to src.model
mv src/models src/model

# Push initial setup
git add .
git commit -m "Initial project setup"
git push origin main
```

### 2️⃣ MLflow Setup on DagsHub
```bash
# Install dependencies
pip install dagshub mlflow

# Create and connect repo on DagsHub
# Copy experiment tracking URL & integrate into the project
```

### 3️⃣ DVC Setup
```bash
# Initialize DVC
dvc init
mkdir local_s3  # Temporary local storage
dvc remote add -d mylocal local_s3
```

### 4️⃣ Implement ML Pipeline
Add the following scripts inside `src/`:
- `logger.py`
- `data_ingestion.py`
- `data_preprocessing.py`
- `feature_engineering.py`
- `model_building.py`
- `model_evaluation.py`
- `register_model.py`

```bash
# Initialize DVC pipeline
dvc repro
# Track changes
git add .
git commit -m "Added ML pipeline"
git push origin main
```

### 5️⃣ AWS S3 Setup for DVC
```bash
# Install dependencies
pip install dvc[s3] awscli

# Configure AWS credentials
aws configure

# Add S3 as remote storage
dvc remote add -d myremote s3://<bucket-name>
```

---

## 🛠️ Deployment Setup
### 6️⃣ Docker Integration
```bash
# Install pipreqs & generate requirements
pip install pipreqs
cd flask_app && pipreqs . --force

# Create & build Docker image
docker build -t capstone-app:latest .

# Run Docker container
docker run -p 8888:5000 capstone-app:latest
```

### 7️⃣ CI/CD Pipeline Setup
- Add `.github/workflows/ci.yaml`
- Generate a DagsHub API token & save it in GitHub Secrets
- Configure AWS credentials for deployment

### 8️⃣ Deploy to AWS EKS
```bash
# Create EKS cluster
eksctl create cluster --name flask-app-cluster --region us-east-1 --nodegroup-name flask-app-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 1 --managed

# Update kubeconfig
aws eks --region us-east-1 update-kubeconfig --name flask-app-cluster

# Deploy application using Kubernetes
git add .
git commit -m "Updated Kubernetes deployment"
git push origin main
```

### 9️⃣ Monitoring with Prometheus & Grafana
#### Prometheus Setup
```bash
# SSH into EC2 instance
ssh -i your-key.pem ubuntu@your-ec2-public-ip

# Install Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz
tar -xvzf prometheus-2.46.0.linux-amd64.tar.gz
mv prometheus-2.46.0.linux-amd64 prometheus

# Move files to standard paths
sudo mv prometheus /etc/prometheus
sudo mv /etc/prometheus/prometheus /usr/local/bin/
```

#### Grafana Setup
```bash
# Install Grafana on EC2 instance
wget https://dl.grafana.com/oss/release/grafana-8.5.0.linux-amd64.tar.gz
tar -xvzf grafana-8.5.0.linux-amd64.tar.gz
mv grafana-8.5.0 /usr/local/grafana
```

---

## 🎯 Final Deployment & Access
- Fetch the LoadBalancer external IP:
  ```bash
  kubectl get svc flask-app-service
  ```
- Access the app:
  ```bash
  curl http://<external-ip>:5000
  ```
- Monitor with Prometheus & Grafana dashboards.

---

## 🎯 Key Technologies Used
- **Machine Learning**: Python, MLflow, DVC
- **Cloud & Storage**: AWS S3, IAM, EKS, EC2
- **Containerization**: Docker, Kubernetes
- **CI/CD**: GitHub Actions, DagsHub, AWS ECR
- **Monitoring**: Prometheus, Grafana

---

## 📌 Contribution
Contributions are welcome! Feel free to open issues and submit pull requests.

---

## 📜 License
This project is licensed under the MIT License.

