# INFO8995A3 Assignment 3 - Orchestration with Gitea

## External ngrok URL

ngrok exposed at: 

## Structure
- `gitea/up.yml`: Ansible playbook to deploy Gitea via Helm
- `gitea/values.yaml`: Gitea Helm values (uses SQLite for simplicity)
- `ngrok/up.yaml`: Kubernetes manifest to expose Gitea via ngrok (not included in prod)
- `gitea/ci-cd-pipeline-example.yml`: Example Gitea Actions workflow

## Instructions

## Step-by-Step Deployment Guide

1. **Install dependencies:**
   ```bash
   pip install ansible kubernetes
   ```

2. **Initialize git submodules (if your repo uses them):**
   ```bash
   git submodule update --init --recursive
   ```

3. **Start Kubernetes cluster (Minikube):**
   ```bash
   minikube start
   ```

4. **Deploy Gitea using Ansible:**
   ```bash
   ansible-playbook up.yml
   ```

5. **Wait until all Gitea pods are running:**
   ```bash
   kubectl get pod
   ```

6. **Port-forward to access Gitea locally:**
   ```bash
   kubectl port-forward svc/gitea-http 3000:3000
   ```

7. **apply ngrok manifest:**
   ```bash
   kubectl apply -f ngrok/up.yaml
   ```

8. **Check ngrok pod and logs:**
   ```bash
   kubectl get pods -n ngrok
   kubectl logs -n ngrok -l app=ngrok
   ```

9. **Visit the ngrok URL:**
   Open the browser and go to to access Gitea externally.



# cdevops-gitea
k8s gitea lab to take dev (sqlite based) to prod (mysql based)

TLDR;

```bash
pip install ansible kubernetes
git submodule update --init --recursive
ansible-playbook up.yml
```

Wait until `kubectl get pod` shows all pods running and:

```bash
kubectl port-forward svc/gitea-http 3000:3000
```

Now you should be able to access gitea in development mode.

The challenge is to run this in production mode.

### Points to Cover

## Marking

|Item|Out Of|
|--|--:|
|use [the gitea helm](https://gitea.com/gitea/helm-gitea) to make the repository data persistent|3|
|make gitea use external database|3|
|Use [this article](https://blog.techiescamp.com/using-ngrok-with-kubernetes/) to expose your gitea instance publically|2|
|make the README easy to use and ACCURATE|2|
|||
|total|10|
