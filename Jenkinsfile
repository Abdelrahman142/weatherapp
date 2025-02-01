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
                        if [ -d "weatherapp" ]; then
                            cd weatherapp
                            git checkout main
                            git pull --rebase origin main
                        else
                            git clone https://${GIT_USER_NAME}:${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git weatherapp
                            cd weatherapp
                            git checkout main
                        fi
                    '''
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                    # Navigate to the Ansible directory
                    cd weatherapp/Ansible

                    # Add SSH keys to the SSH agent
                    eval $(ssh-agent -s)
                    ssh-add ~/.ssh/private_key1
                    ssh-add ~/.ssh/private_key2

                    # Run the Ansible playbook
                    ansible-playbook -i inventory docker-deploy.yml
                '''
            }
        }
    }
}




        // stage('Build Docker Image') {
        //     steps {
        //         sh '''
        //             cd weatherapp
        //             docker build -t ${DOCKER_IMAGE} .
        //         '''
        //     }
        // }

        // stage('Push Docker Image') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
        //             sh '''
        //                 echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        //                 docker push ${DOCKER_IMAGE}
        //             '''
        //         }
        //     }
        // }

    
