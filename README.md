# 🚗 MLOps Vehicle Insurance Data Pipeline

End-to-end **MLOps pipeline** for Vehicle Insurance Prediction, covering **data ingestion → validation → transformation → model training → evaluation → deployment**, with **MongoDB, AWS, Docker, CI/CD, and Flask**.

---

## 📌 Project Overview

This project demonstrates a **complete MLOps lifecycle**:

* Modular Python package structure
* MongoDB Atlas as data source
* Robust logging & exception handling
* Data validation & transformation pipelines
* Model training, evaluation, and versioning
* AWS S3 for model registry
* CI/CD using GitHub Actions
* Dockerized deployment on AWS EC2
* Flask-based prediction API
* Set up TLS with DNS and nginx to enable https

---

## 🏗️ Project Setup & Initialization

### 1️⃣ Create Project Template

Generate the base project structure:

```bash
python template.py
```

---

### 2️⃣ Configure Local Package Imports

Edit the following files to enable local package imports:

* `setup.py`
* `pyproject.toml`

📄 Reference: `crashcourse.txt` (explains `setup.py` & `pyproject.toml`)

---

### 3️⃣ Create Virtual Environment & Install Dependencies

```bash
conda create -n mlops-vehicle python=3.10 -y
conda activate mlops-vehicle
pip install -r requirements.txt
```

📌 Add all required libraries to `requirements.txt`

---

### 4️⃣ Verify Installed Packages

```bash
pip list
```

Ensure your **local project package** is listed.

---

## 🍃 MongoDB Atlas Setup

### 5️⃣ Create MongoDB Project

* Sign up at **MongoDB Atlas**
* Create a new project → *Next → Next → Create*

---

### 6️⃣ Create Cluster

* Choose **M0 (Free Tier)**
* Keep defaults → Create Deployment

---

### 7️⃣ Create Database User

* Set username & password
* Save credentials securely

---

### 8️⃣ Configure Network Access

Allow global access:

```text
0.0.0.0/0
```

---

### 9️⃣ Get Connection String

* Go to **Connect → Drivers**
* Select:

  * Driver: Python
  * Version: 4.7+
* Copy connection string
* Replace `<password>`

---

### 🔬 MongoDB Notebook Demo

### 🔟 Setup Notebook

```text
notebook/
 └── mongoDB_demo.ipynb
```

* Select Kernel → `mlops-vehicle`
* Add dataset to `notebook/`

---

### 1️⃣1️⃣ Push Dataset to MongoDB

* Load dataset in notebook
* Push data to MongoDB
* Verify at:

```text
MongoDB Atlas → Database → Browse Collections
```

---

## 🧾 Logging, Exceptions & EDA

### 1️⃣2️⃣ Logging

* Implement logger module
* Test using `demo.py`

### 1️⃣3️⃣ Custom Exceptions

* Implement exception handling
* Test using `demo.py`

### 1️⃣4️⃣ EDA & Feature Engineering

* Add notebooks for:

  * Exploratory Data Analysis
  * Feature Engineering

---

## 📥 Data Ingestion Pipeline

### 1️⃣5️⃣ Configuration & Setup

Before ingestion:

* Define constants in `constants/__init__.py`
* Implement MongoDB connection in:

  ```text
  configuration/mongo_db_connections.py
  ```
* Fetch & transform data in:

  ```text
  data_access/proj1_data.py
  ```

---

### 1️⃣6️⃣ Entity Definitions

* `entity/config_entity.py` → `DataIngestionConfig`
* `entity/artifact_entity.py` → `DataIngestionArtifact`

---

### 1️⃣7️⃣ Data Ingestion Component

* Implement `components/data_ingestion.py`
* Add to training pipeline
* Run:

```bash
python demo.py
```

---

### 1️⃣8️⃣ Setup MongoDB Environment Variable

#### Bash (Mac/Linux/WSL)

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>..."
echo $MONGODB_URL
```

#### PowerShell (Windows)

```powershell
$env:MONGODB_URL="mongodb+srv://<username>:<password>..."
echo $env:MONGODB_URL
```

📌 Add `artifact/` to `.gitignore`

---

## ✅ Data Validation, Transformation & Training

### 1️⃣9️⃣ Utilities & Schema

* Complete:

  * `utils/main_utils.py`
  * `config/schema.yaml`

---

### 2️⃣0️⃣ Data Validation

Implement pipeline similar to Data Ingestion:

* Config
* Artifact
* Component
* Pipeline integration

---

### 2️⃣1️⃣ Data Transformation

* Add `estimator.py` in `entity/`
* Implement transformation logic

---

### 2️⃣2️⃣ Model Trainer

* Extend `estimator.py`
* Train & save model

---

## ☁️ AWS Setup (Model Registry)

### 2️⃣3️⃣ AWS Configuration

#### IAM User

* Name: `Vehicle_insurance`
* Policy: `AdministratorAccess`
* Generate Access Keys (CSV)

---

#### Set AWS Environment Variables

**Bash**

```bash
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
```

**PowerShell**

```powershell
$env:AWS_ACCESS_KEY_ID="..."
$env:AWS_SECRET_ACCESS_KEY="..."
```

---

### Constants Required

```python
MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE = 0.02
MODEL_BUCKET_NAME = "mlops-vehicle-insurance-data-pipeline"
MODEL_PUSHER_S3_KEY = "model-registry"
```

---

### S3 Bucket

* Name: `mlops-vehicle-insurance-data-pipeline`
* Region: `us-east-1`
* Disable block public access

---

### AWS Storage Layer

* Implement:

  ```text
  cloud_storage/aws_storage.py
  entity/s3_estimator.py
  ```

---

## 🧪 Model Evaluation & Pusher

### 2️⃣4️⃣ Implement:

* Model Evaluation
* Model Pusher (S3 upload)

---

## 🔮 Prediction Pipeline

### 2️⃣5️⃣ Setup Prediction Pipeline

* Create prediction modules
* Implement `app.py`

---

### 2️⃣6️⃣ Frontend Assets

```text
static/
templates/
```

---

## 🚀 CI/CD & Deployment

### 2️⃣7️⃣ Docker & GitHub Actions

* Create:

  * `Dockerfile`
  * `.dockerignore`
* Add:

  ```text
  .github/workflows/aws.yaml
  ```

---

### AWS ECR

* Repo name:

```text
mlops-vehicle-insurance-data-pipeline
```

---

### EC2 Setup

* Ubuntu 24.04
* Instance: `t2.medium`
* Open HTTP/HTTPS
* Storage: 30GB

---

### 2️⃣8️⃣ Install Docker on EC2

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

---

### 2️⃣9️⃣ GitHub Self-Hosted Runner

* Add runner from GitHub → Actions → Runners
* Run setup commands on EC2
* Verify runner is **Idle**

---

### 3️⃣0️⃣ GitHub Secrets

Add:

* `AWS_ACCESS_KEY_ID`
* `AWS_SECRET_ACCESS_KEY`
* `AWS_DEFAULT_REGION`
* `ECR_REPO`

---

### 3️⃣1️⃣ CI/CD Trigger

* Pipeline triggers on **git push**

---

### 3️⃣2️⃣ Open Port 5000

* EC2 Security Group → Inbound Rule

```text
TCP | 5000 | 0.0.0.0/0
```

---

### 3️⃣3️⃣ Launch Application

```text
http://<EC2_PUBLIC_IP>:5000
```

---

### 3️⃣4️⃣ Model Training Route

```text
http://<EC2_PUBLIC_IP>:5000/training
```

---

## ✅ Final Notes

* This project follows **industry-grade MLOps architecture**
* Highly scalable & production-ready
* Easy to extend for monitoring & retraining

---

## 🔐 HTTPS Setup with Nginx (Reverse Proxy)

To secure application traffic, the project is deployed behind an **Nginx reverse proxy** with **TLS/SSL enabled**, ensuring all client communication happens over **HTTPS**.

---

### 📌 Architecture Flow

```text
Client (Browser)
      │
   HTTPS (443)
      │
   Nginx (TLS Termination)
      │
   HTTP (5000)
      │
   Flask App (Prediction API)
```

---

### ⚙️ Nginx Deployment

* Nginx runs as a **Docker container**
* Acts as a **reverse proxy** for the Flask application
* Handles:

  * TLS/SSL termination
  * Secure traffic routing
  * HTTP → HTTPS redirection (optional)

---

### 🔑 TLS / SSL Configuration

* TLS certificates configured inside the Nginx container
* Certificates mounted using Docker volumes
* HTTPS traffic exposed on **port 443**
* Internal application traffic forwarded to **port 5000**

Example Nginx behavior:

```text
https://your-domain.com  →  nginx:443  →  flask-app:5000
```

---

### 🐳 Docker-Based Setup

* Nginx container runs alongside the application container
* Nginx config includes:

  * `ssl_certificate`
  * `ssl_certificate_key`
  * `proxy_pass` to backend app
  * Secure headers

---

### 🌐 Domain & DNS

* Domain configured to point to **EC2 public IP**
* DNS ensures HTTPS traffic reaches the Nginx container
* TLS validation successfully completed

---

### ✅ Security Benefits

* Encrypted data in transit
* No direct public exposure of Flask application
* Production-ready reverse proxy setup
* Improved compliance and trust

---

### 🔍 Verification

You can verify HTTPS setup using:

```bash
curl -I https://your-domain.com
```

Or via browser:

* 🔒 Lock icon confirms secure TLS connection

---

### 📌 Notes

* Flask app listens only on internal port (`5000`)
* Public traffic is handled exclusively by Nginx
* Easily extendable to:

  * Rate limiting
  * Authentication
  * WAF integration


