pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abdelrahmangazy/weatherapp:latest"
        GIT_REPO_NAME = "weatherapp"
        GIT_USER_NAME = "Abdelrahman142"
    }

    stages {  
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-repo', url: 'https://github.com/Abdelrahman142/weatherapp.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
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

        // stage('Deploy with Ansible') {
        //     steps {
        //         sh '''
        //             # Navigate to the Ansible directory
        //             cd Ansible
        //             # Run the Ansible playbook
        //             ansible-playbook -i inventory docker-deploy.yml
        //         '''
        //     }
        // }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    kubectl rollout restart deployment/weather-app
                '''
            }
        }
    }
}

post {
    success {
        emailext subject: " Jenkins Build Successful: ${env.JOB_NAME}",
                 body: "The Jenkins build for ${env.JOB_NAME} completed successfully. \nBuild URL: ${env.BUILD_URL}",
                 to: "your_email@example.com"
    }
    failure {
        emailext subject: " Jenkins Build Failed: ${env.JOB_NAME}",
                 body: "The Jenkins build for ${env.JOB_NAME} has failed. \nCheck the logs here: ${env.BUILD_URL}",
                 to: "your_email@example.com"
    }
}

