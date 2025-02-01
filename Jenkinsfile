pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abdelrahmangazy/weatherapp:${BUILD_NUMBER}"
        GIT_REPO_NAME = "weatherapp"
        GIT_USER_NAME = "Abdelrahman142"
    }

    stages {
        stage('Clone Repository') {
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git clone https://${GIT_USER_NAME}:${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git weatherapp
                        cd weatherapp
                        git checkout main
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    cd weatherapp
                    docker build -t ${DOCKER_IMAGE} .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                    sh '''
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                    cd weatherapp/Ansible
                    ansible-playbook -i inventory docker-deploy.yml
                '''
            }
        }
    }
}
