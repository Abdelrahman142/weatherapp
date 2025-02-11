pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abdelrahmangazy/weatherapp:latest"
        GIT_REPO_NAME = "weatherapp"
        GIT_USER_NAME = "Abdelrahman142"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[
                            url: "https://github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git",
                            credentialsId: 'github'
                        ]]
                    ])
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
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                    sh '''
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    kubectl rollout restart deployment/weather-app
                '''
            }
        }

        // Uncomment this if you want to deploy with Ansible
        // stage('Deploy with Ansible') {
        //     steps {
        //         sh '''
        //             # Navigate to the Ansible directory
        //             cd weatherapp/Ansible
        //             # Run the Ansible playbook
        //             ansible-playbook -i inventory docker-deploy.yml
        //         '''
        //     }
        // }
    }
}
