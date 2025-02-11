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

🖥️ Running on Jenkins CI/CD
Pipeline Stages:
Clone Repository: Fetch the latest code.
Build Docker Image: Create an image from the Flask app.
Push to Docker Hub: Upload the image for deployment.
Deploy with Ansible: Automate deployment on Vagrant instances.
To trigger the pipeline manually:
```bash
jenkins build weatherapp
```

You can check running containers with:
```bash
docker ps
```


