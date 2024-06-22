pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-west-1' // Change this to your AWS region if different
        AWS_ACCOUNT_ID = '381491824886' // Replace with your 12-digit AWS account ID
        IMAGE_REPO_NAME = 'flask-app'
        IMAGE_TAG = "latest"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Ensure you have added DockerHub credentials in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/GLcoz/flask-app-python.git' // Ensure this is the correct URL of your GitHub repository
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Building the Docker image
                    sh 'docker build -t flask-app:latest .'
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    // Logging into DockerHub and pushing the image
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    sh 'docker tag flask-app:latest your_dockerhub_username/flask-app:latest' // Replace with your DockerHub username
                    sh 'docker push your_dockerhub_username/flask-app:latest' // Replace with your DockerHub username
                }
            }
        }

        stage('Deploy to ECS') {
            steps {
                script {
                    sh '''
                    aws ecs update-service --cluster your-cluster-name --service your-service-name --force-new-deployment
                    ''' // Replace 'your-cluster-name' and 'your-service-name' with your actual ECS cluster name and service name
                }
            }
        }
    }
}
