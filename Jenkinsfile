pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-cred')
        GIT_CREDENTIALS = credentials('github')
        DOCKER_IMAGE = 'abdelrahmangazy/weathewrapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/Abdelrahman142/weatherapp.git weatherapp'
                sh 'cd weatherapp'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'cd weatherapp/Ansible'

                sh 'ansible-playbook -i inventory docker-deploy.yml'
            }
        }
    }
}

