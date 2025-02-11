# WeatherApp 🌤️

A **Flask-based** weather application that fetches real-time weather data from OpenWeatherMap and stores search history in **SQLite**.  
This project is fully containerized using **Docker**, deployed via **Jenkins**, and orchestrated with **Ansible** on **Vagrant VMs**.  
Additionally, the application is now integrated with **Minikube** for Kubernetes deployment.

## 🚀 Features
✅ Get real-time weather information for any city.  
✅ Store last searched city in an **SQLite database**.  
✅ Deployable via **Docker & Jenkins CI/CD**.  
✅ Automated setup using **Ansible & Vagrant**.  
✅ **Kubernetes support** using Minikube.  

## 🛠️ Technologies Used
- **Backend**: Flask (Python)
- **Database**: SQLite
- **Containerization**: Docker
- **Automation**: Jenkins, Ansible, Vagrant
- **Orchestration**: Kubernetes (Minikube)
- **Deployment**: Vagrant VMs / Minikube

## 📦 Setup Instructions

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/Abdelrahman142/weatherapp.git
cd weatherapp

---

## 📦 Setup Instructions

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/Abdelrahman142/weatherapp.git
cd weatherapp
```
2️⃣ Run with Docker

```bash
docker build -t abdelrahmangazy/weatherapp:latest .
docker run -d -p 5000:5000 --name weatherapp abdelrahmangazy/weatherapp:latest
```
3️⃣ Deploy with Ansible (on Vagrant VMs)
```bash
cd Ansible
ansible-playbook -i inventory docker-deploy.yml
```

Running on Jenkins CI/CD
Pipeline Stages:

1️⃣ Checkout Repository → Fetch the latest code.
2️⃣ Build Docker Image → Create an image from the Flask app.
3️⃣ Push to Docker Hub → Upload the image for deployment.
4️⃣ Deploy to Minikube → Restart the deployment for the latest changes.
5️⃣ Deploy with Ansible (on Vagrant VMs)
To trigger the pipeline manually:
```bash
jenkins build weatherapp
```

You can check running containers with:
```bash
docker ps
```
Running on Minikube
1️⃣ Start Minikube
```bash

minikube start
```
2️⃣ Enable Docker inside Minikube
```bash

eval $(minikube docker-env)
```
3️⃣ Build the Docker Image inside Minikube
```bash

docker build -t abdelrahmangazy/weatherapp:latest .
```
4️⃣ Apply Kubernetes Configurations
```bash

kubectl apply -f deployment.yaml
kubectl apply -f weather-app-service.yaml
```
5️⃣ Expose the Service
```bash

minikube service weather-app --url
```
This will return a URL that you can open in your browser to access the weather app.
6️⃣ Check Running Pods
```bash

kubectl get pods
```
7️⃣ Restart Deployment if Needed
```bash

kubectl rollout restart deployment/weather-app
```
🖥️ Running on Jenkins CI/CD
Pipeline Stages:

1️⃣ Checkout Repository → Fetch the latest code.
2️⃣ Build Docker Image → Create an image from the Flask app.
3️⃣ Push to Docker Hub → Upload the image for deployment.
4️⃣ Deploy to Minikube → Restart the deployment for the latest changes.
5️⃣ Deploy with Ansible (on Vagrant VMs).

To trigger the pipeline manually:
```bash

jenkins build weatherapp
```
🔍 Useful Commands

Check running containers:
```bash

kubectl get services
```
Check logs of a running pod:
```bash

kubectl logs -f <pod-name>
```
