pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nawaz1114/flask-stylish-app'
        IMAGE_TAG = 'v2'
        DOCKER_CREDENTIALS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:$IMAGE_TAG ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: "$DOCKER_CREDENTIALS_ID", url: ""]) {
                    sh "docker push $DOCKER_IMAGE:$IMAGE_TAG"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 5001:5000 $DOCKER_IMAGE:$IMAGE_TAG"
            }
        }
    }
}
