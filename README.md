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
![Screenshot from 2025-02-11 23-12-34](https://github.com/user-attachments/assets/05beea20-fa84-4735-b773-80b985d7cc16)

7️⃣ Restart Deployment if Needed
```bash

kubectl rollout restart deployment/weather-app
```
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
![Screenshot from 2025-02-11 23-11-44](https://github.com/user-attachments/assets/d75bbcda-9a09-49ee-b50f-ac552e921cf1)

Email Notifications in Jenkins

This Jenkins pipeline is configured to send email notifications on build success or failure.

    ✅ On Success: An email is sent with a subject:
    "Jenkins Build Successful: weatherapp"

![Screenshot from 2025-02-11 23-16-19](https://github.com/user-attachments/assets/f233559f-4aa6-4f05-86ad-150d58e3a84e)

    ❌ On Failure: An email is sent with a subject:
    "Jenkins Build Failed: weatherapp"
    
![Screenshot from 2025-02-11 23-16-35](https://github.com/user-attachments/assets/a8706d52-e70f-44e6-8169-6359b9d398e0)

Continuous Deployment with ArgoCD

This project uses ArgoCD for continuous deployment on a Kubernetes cluster.
🔧 Setup Instructions:

    Install ArgoCD on your Kubernetes cluster:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
Access the ArgoCD UI:
```bash

kubectl port-forward svc/argocd-server -n argocd 8080:443

Then, open https://localhost:8080.
```
Login to ArgoCD:
```bash

argocd login localhost:8080
```
Deploy the application with ArgoCD:
```bash

argocd app create weatherapp \
  --repo https://github.com/Abdelrahman142/weatherapp.git \
  --path kubernetes \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
```
![Screenshot from 2025-02-11 23-11-01](https://github.com/user-attachments/assets/68efcc8c-d7f2-42fd-9308-acde5a5710b3)

Sync the application:
```bash

    argocd app sync weatherapp
```
📌 ArgoCD Benefits in this project:

✅ Automated GitOps deployment
✅ Rollback & Version control
✅ Real-time monitoring

🔧 Configuration Steps:

    Ensure Jenkins has the Email Extension Plugin installed.

    Go to Manage Jenkins → Configure System → Extended E-mail Notification and configure SMTP settings.

    The pipeline includes the following post block:

  ```bash
post {
    success {
        emailext (
            subject: "✅ Jenkins Build Successful: ${env.JOB_NAME}",
            body: "The Jenkins build for ${env.JOB_NAME} completed successfully. 🎉\nBuild URL: ${env.BUILD_URL}",
            to: "abdodabos11@gmail.com"
        )
    }
    failure {
        emailext (
            subject: "❌ Jenkins Build Failed: ${env.JOB_NAME}",
            body: "The Jenkins build for ${env.JOB_NAME} has failed. ⚠️\nCheck the logs here: ${env.BUILD_URL}",
            to: "abdodabos11@gmail.com"
        )
    }
}
```
