# Docker Monitoring Project – Uptime-Kuma Deployment
**Author:** Dillon Stinson  
**Course:** Systems Administration  
**Project:** Deploying a Docker-Based Monitoring Service

---

## 1. Overview

This project demonstrates the deployment of a self-hosted monitoring service using Docker and Docker Compose.  
The goal was to configure a virtual machine, install Docker, set up project directories, deploy **Uptime-Kuma**, and configure website monitors.

---

## 2. Virtual Machine Setup

I created an Ubuntu-based virtual machine and resolved an initial issue where:

`failed to start docker.service`


After rebooting and troubleshooting, the VM successfully loaded into:
```
cyber-system-admin login:
```

### **System checks performed**
```bash
systemctl status docker
```
Docker was confirmed to be running.

## 3. Installing Docker Engine

### 3.1 Install prerequisite packages
```
sudo apt update
sudo apt install ca-certificates curl gnupg
```
### 3.2 Add Docker’s official GPG key
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
### 3.3 Add Docker repository
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### 3.4 Install Docker Engine + Compose Plugin
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### 3.5 Enable and start the Docker service
```
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

## 4. Creating the Project Directory

All Docker projects are kept in a dedicated directory:
```
mkdir -p ~/docker-projects/uptime-kuma
cd ~/docker-projects/uptime-kuma
```
## 5. Docker-Compose Configuration

The required docker-compose.yml file was created using nano

docker-compose.yml contents:
```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    ports:
      - "3001:3001"
    volumes:
      - uptime-kuma-data:/app/data
    restart: always

volumes:
  uptime-kuma-data:
```
Data is stored in a Docker volume, and the service is configured to use port 3001 and restart automatically.

## 6. Launching Uptime-Kuma

```
sudo docker compose up -d
```
verify it is running
```
docker ps
```
**Screenshot Placeholder**
![Docker PS](images/docker-ps.png)

## 7. Accessing the Dashboard

`http://127.0.0.1:3001`

**Screenshot Placeholder**
![Uptime Kuma Dashboard](images/dashboard.png)

## 8. Adding Monitoring Services

I configured two monitors for testing:

- Google (HTTP)
- GitHub (HTTP)

