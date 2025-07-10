# 🧪 CI/CD Pipeline with Trivy, CodeQL, SonarCloud & Docker Deploy. 

This repository contains a robust CI/CD pipeline for a **Jekyll static site** using **GitHub Actions**. The workflow includes security scans, code analysis, Docker image management, and automatic deployment to an **EC2 instance**.

---

## 📋 Workflow Overview

### 🔐 1. Secret Scan with Gitleaks
- Scans for hardcoded secrets or sensitive data.
- Helps enforce security best practices.

### 🔍 2. CodeQL Static Analysis
- Performs static code analysis for JavaScript.
- Identifies potential security vulnerabilities and code issues.

### 🧪 3. SonarCloud Scan
- Performs code quality and maintainability analysis via **SonarScanner**.
- Uses [SonarCloud Dashboard](https://sonarcloud.io/dashboard?id=YOUR_PROJECT_KEY)  
  *(Replace `YOUR_PROJECT_KEY` with your actual project key)*
- Triggered using secrets:
  - `SONAR_TOKEN`
  - `SONAR_PROJECT_KEY`
  - `SONAR_ORG`

### 🧱 4. Build Jekyll Site
- Builds the static site using `jekyll/builder` Docker image.

### 🛡️ 5. Trivy Image Scan
- Scans the `jekyll/builder` Docker image for vulnerabilities.
- Fails the pipeline if **CRITICAL/HIGH** severity issues are found.

### 📦 6. Docker Build & Push
- Builds Docker image from repo source.
- Pushes to **Docker Hub** using:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN`

### 🚀 7. Deploy to EC2
- SSHs into EC2 using:
  - `SERVER_IP`, `EC2_USER`, and `SERVER_KEY`
- Pulls and runs the latest Docker image.

---

## 🗓️ Trigger Types

- **On Push** to `main`
- **On Pull Request** to `main`
- **Scheduled**: Every Monday at 2 AM UTC

---

## 🔐 Required GitHub Secrets

| Secret Name             | Description                         |
|-------------------------|-------------------------------------|
| `SONAR_TOKEN`           | SonarCloud authentication token     |
| `SONAR_PROJECT_KEY`     | SonarCloud project key              |
| `SONAR_ORG`             | SonarCloud organization             |
| `DOCKERHUB_USERNAME`    | Docker Hub username                 |
| `DOCKERHUB_TOKEN`       | Docker Hub access token             |
| `SERVER_IP`             | Public IP of EC2 instance           |
| `EC2_USER`              | SSH user for EC2                    |
| `SERVER_KEY`            | Private key for EC2 (Base64 or PEM) |

---

## 🚀 Deployment Info

- Docker container is exposed on **port 80** on EC2.
- Old container is stopped and replaced on every deploy.

---

## 📂 Directory Structure

```bash
.
├── .github/
│   └── workflows/
│       └── main.yml      # CI/CD workflow
├── _site/                # Jekyll build output (auto-generated)
├── Dockerfile            # Custom Docker build (if any)
├── README.md             # You're reading it!
