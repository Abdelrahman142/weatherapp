# WeatherApp 🌤️

A **Flask-based** weather application that fetches real-time weather data from OpenWeatherMap and stores search history in **SQLite**.  
This project is fully containerized using **Docker**, deployed via **Jenkins**, and orchestrated with **Ansible** on **Vagrant VMs**.  
Additionally, the application is now integrated with **Minikube** for Kubernetes deployment.
##
![Blank diagram(3)](https://github.com/user-attachments/assets/cd9ca509-da4a-4f21-9eca-4fcb0e73b484)
##
## 🚀 Features
✅ Get real-time weather information for any city.  
✅ Store last searched city in an **SQLite database**.  
✅ Deployable via **Docker & Jenkins CI/CD**.  
✅ Automated setup using **Ansible & Vagrant**.  
✅ **Kubernetes support** using Minikube.  
✅ **NGINX** to act as a  load blancer.  


## 🛠️ Technologies Used
- **Backend**: Flask (Python)
- **Database**: SQLite
- **Containerization**: Docker
- **Automation**: Jenkins, Ansible, Vagrant
- **Orchestration**: Kubernetes (Minikube)
- **NGINX**: Load Balancer
- **Deployment**: Vagrant VMs / Minikube

```
```
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

Running on Jenkins CI/CD
##Pipeline Stages:

- 1️⃣ Checkout Repository → Fetch the latest code.
- 2️⃣ Build Docker Image → Create an image from the Flask app.
- 3️⃣ Push to Docker Hub → Upload the image for deployment.
- 4️⃣ Deploy to Minikube → Restart the deployment for the latest changes.
- 5️⃣ Deploy with Ansible (on Vagrant VMs)

Jenkins Deployment

Jenkins is used for automating the build and deployment process.

1. Install Jenkins
```bash
sudo apt update && sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
echo "deb http://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt update && sudo apt install jenkins -y
sudo systemctl enable --now jenkins
```

To trigger the pipeline manually: Go to 
```bash
localhost:8080
```
and create a new pipline and configere it
- Add your ceridintal in to jenkins and use replace the jenkinsfile with your id
- then click Build Now
  
![Screenshot from 2025-02-12 00-55-28](https://github.com/user-attachments/assets/2b04e457-2799-4681-ba2f-c60fefea6be5)


You can check running containers with:
```bash
docker ps
```
📦 Local Development with Vagrant

This project supports Vagrant to set up a local development environment with VirtualBox.
🔧 Setup Instructions:
``
    Install Vagrant & VirtualBox:
        Download and install Vagrant
        Download and install VirtualBox

Create the Vagrant environment and Edit it as you Like:
```
vagrant init
```
Then

```bash
vagrant up
```
Access the virtual machine:
```bash

vagrant ssh
```

📌 Vagrant Benefits in this project:

- ✅ Easy setup for local development
- ✅ Consistent development environment
- ✅ Pre-configured dependencies


Deploy with Ansible

This project uses Ansible for automated deployment.
📌 Prerequisites:
``
    Install Ansible (if not installed):
```
sudo apt update && sudo apt install ansible -y
```
Configure your inventory file:
`
    Edit Ansible/inventory and add your server's IP:
```
    [web]
    your_server_ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```
⚡ Deployment Steps:
``
    Clone the repository:
```
git clone https://github.com/Abdelrahman142/weatherapp.git
cd weatherapp/Ansible
```
Run the Ansible playbook:
Note: verify the owner of private keys is jenkins to can access it by jenkins 
```
ansible-playbook -i inventory docker-deploy.yml
```
- you can change owner by this commaned:
  ```
  sudo chown jenkins:jenkins <path to file>
  sudo chmod 600 <path to file>
  ```
![Screenshot from 2025-02-12 00-22-35](https://github.com/user-attachments/assets/b86f6a04-2123-4a2a-8eaa-a67b2bafa78f)

Verify the deployment:
```
    http://192.168.56.10:5000
```
![Screenshot from 2025-02-12 00-11-14](https://github.com/user-attachments/assets/65e5fcb2-0fbc-42ef-bfce-bc79de4a7c2b)

    http://192.168.56.11:5000
![Screenshot from 2025-02-12 00-44-08](https://github.com/user-attachments/assets/20f8582e-5f12-41c3-a512-888edb5188a6)


```
```
📌 Benefits of Ansible Deployment:

✅ Automated setup & deployment
✅ Scalable infrastructure
✅ Easier server configuration


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
minikube addons enable ingress
kubectl apply -f deployment.yaml   #application
kubectl apply -f weather-app-service.yaml   #serviec
kubectl applu -f weather-ingress.yaml   # Nginx
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

🔍 Useful Commands

Check running containers:
```bash

kubectl get services
```
Check logs of a running pod:
```bash

kubectl logs -f <pod-name>
```
Chack ingress Service Working 
Go to 
```
weather.locl
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
```
Note: IF YOU RUN JENKENS IN PORT 8080 CHANGE PORT FORWARD TO 8081 OR ANY OTHER PORT

```
Then, open https://localhost:8080.
```
Login to ArgoCD:
```bash

argocd login localhost:8080
```
3. Log in to ArgoCD

Retrieve initial password:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
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

-   Ensure Jenkins has the Email Extension Plugin installed.

-   Go to Manage Jenkins → Configure System → Extended E-mail Notification and configure SMTP settings.
-   Go to Jenkins Dashboard → Manage Jenkins → Configure System.
Under "Extended E-mail Notification":

    SMTP Server: Set it to smtp.gmail.com
    Use SMTP Authentication: ✅ Checked
       - Username: your-email@gmail.com
       - Password: (Use an App Password if 2FA is enabled)
    Use SSL: ✅ Checked
    - SMTP Port: 465
   - Reply-To Address: your-email@gmail.com
    Charset: UTF-8
    Click Save.

-    The pipeline includes the following post block:
-    replace  example@.com with your email
    
     
  ```bash
post {
    success {
        emailext (
            subject: "✅ Jenkins Build Successful: ${env.JOB_NAME}",
            body: "The Jenkins build for ${env.JOB_NAME} completed successfully. 🎉\nBuild URL: ${env.BUILD_URL}",
            to: "example@.com"
        )
    }
    failure {
        emailext (
            subject: "❌ Jenkins Build Failed: ${env.JOB_NAME}",
            body: "The Jenkins build for ${env.JOB_NAME} has failed. ⚠️\nCheck the logs here: ${env.BUILD_URL}",
            to: "example@.com"
        )
    }
}
```
