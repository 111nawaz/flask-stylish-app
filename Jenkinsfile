pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nawaz1114/flask-stylish-app'
        IMAGE_TAG = 'v2'
        DOCKER_CREDENTIALS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/111nawaz/flask-stylish-app.git'
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
                // Stop and remove existing container if exists
                sh "docker rm -f flask-container || true"
                // Run the new container
                sh "docker run -d -p 5001:5000 --name flask-container $DOCKER_IMAGE:$IMAGE_TAG"
            }
        }
    }
}
