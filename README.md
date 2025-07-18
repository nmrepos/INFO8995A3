# INFO8995A3 Assignment 3 - Orchestration with Gitea

## Overview

This project demonstrates deploying Gitea on Kubernetes using Ansible and Helm, with CI/CD and public exposure via ngrok. It supports both local (sqlite) and production (mysql) database backends.

---

## Repository Structure

- `gitea/up.yml`: Ansible playbook to deploy Gitea via Helm
- `gitea/values.yaml`: Helm values (default: sqlite, in-memory session/cache)
- `gitea/ci-cd-pipeline-example.yml`: Example Gitea Actions workflow
- `ngrok/up.yaml`: Kubernetes manifest to expose Gitea via ngrok
- `k8s/`: Additional k3d/k3s cluster setup scripts

---

## Prerequisites

- Python 3.x
- Ansible
- Kubernetes cluster (e.g., Minikube, k3d, or k3s)
- kubectl
- Helm
- ngrok account (for public exposure)

---

## ngrok Token Setup

**Important:** the ngrok authtoken is not included in this repository.

Before running the ngrok manifest, you must add your own ngrok authtoken:

1. Get your authtoken from https://dashboard.ngrok.com/get-started/your-authtoken
2. Base64-encode your token:
   ```bash
   echo -n 'YOUR_NGROK_AUTHTOKEN' | base64
   ```
3. Edit `ngrok/up.yaml` and replace `REPLACE_WITH_BASE64_NGROK_TOKEN` with your base64-encoded token in the Secret section.
---
## Quick Start (Development Mode)

```bash
pip install ansible kubernetes
git submodule update --init --recursive
minikube start  # or your preferred k8s cluster
ansible-playbook gitea/up.yml
kubectl get pods
kubectl port-forward svc/gitea-http 3000:3000
```

Visit [http://localhost:3000](http://localhost:3000) to access Gitea.

---

## Exposing Gitea Publicly with ngrok

1. **Apply the ngrok manifest:**
   ```bash
   kubectl apply -f ngrok/up.yaml
   ```

2. **Get the ngrok pod logs to find your public URL:**
   ```bash
   kubectl logs -n ngrok -l app=ngrok
   ```
   Look for a line starting with `https://...ngrok-free.app`.

   > If you see an error about too many agent sessions, end old sessions in your ngrok dashboard and delete error pods.

3. **Open the ngrok URL in your browser to access Gitea from anywhere.**

---

## CI/CD Example

- See `gitea/ci-cd-pipeline-example.yml` for a sample Gitea Actions workflow.
- Place this file in your Gitea repo as `.gitea/workflows/ci.yml`.

---

## Production Mode

- For production, update `gitea/values.yaml` to use a persistent database (e.g., MySQL) and enable persistence.
- See [Gitea Helm Chart](https://gitea.com/gitea/helm-gitea) for configuration options.

---


## Step-by-Step Screenshots

Below are screenshots for each major step of the deployment process.

1. **Install dependencies**
   
   ![Install dependencies](screenshots/Screenshot%20(1059).png)

2. **Initialize git submodules & Starting Kubernetes cluster**
   
   ![Initialize submodules & Start cluster](screenshots/Screenshot%20(1060).png)

4. **Deploy Gitea using Ansible**
   
   ![Deploy Gitea](screenshots/Screenshot%20(1061).png)

5. **Check Gitea pods**
   
   ![Check pods](screenshots/Screenshot%20(1062).png)

6. **Port-forward to access Gitea locally**
   
   ![Port-forward](screenshots/Screenshot%20(1063).png)

7. **Access Gitea Web UI Locally**
   
   ![Gitea Web UI Locally](screenshots/Screenshot%20(1064).png)

7. **Apply ngrok manifest**
   
   ![Apply ngrok](screenshots/Screenshot%20(1065).png)

8. **Check ngrok agent details**
   
   ![ngrok logs](screenshots/Screenshot%20(1066).png)

9. **Access the ngrok endpoints**
   
   ![ngrok URL](screenshots/Screenshot%20(1067).png)

10. **Access Gitea via ngrok public URL**
   
   ![Gitea UI](screenshots/Screenshot%20(1068).png)

---