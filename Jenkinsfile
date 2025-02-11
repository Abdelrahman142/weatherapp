pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abdelrahmangazy/weatherapp:latest"
        GIT_REPO_NAME = "weatherapp"
        GIT_USER_NAME = "Abdelrahman142"

    }

    pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/Abdelrahman142/weatherapp.git', branch: 'main'
            }
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



        
//         stage('Deploy with Ansible') {
//             steps {
//                 sh '''
//                     # Navigate to the Ansible directory
//                     cd weatherapp/Ansible
//                     # Run the Ansible playbook
//                     ansible-playbook -i inventory docker-deploy.yml
//                 '''
//             }
//         }
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

       

    
