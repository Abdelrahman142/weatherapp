pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abdelrahmangazy/weatherapp:latest"
        GIT_REPO_NAME = "weatherapp"
        GIT_USER_NAME = "Abdelrahman142"
        RECIPIENT_EMAIL = 'abdodabos11@gmail.com'
    }

    stages {  
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-repo', url: 'https://github.com/Abdelrahman142/weatherapp.git', branch: 'main'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh '''
                    kubectl rollout restart deployment/weather-app
                '''
            }
        }
    }

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
}
