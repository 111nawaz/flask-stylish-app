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
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/111nawaz/flask-stylish-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t $DOCKER_IMAGE:$IMAGE_TAG ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing Image to Docker Hub..."
                withDockerRegistry([credentialsId: "$DOCKER_CREDENTIALS_ID", url: ""]) {
                    sh "docker push $DOCKER_IMAGE:$IMAGE_TAG"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container..."
                // Stop any existing container with same name
                sh "docker rm -f flask-app || true"
                // Run app
                sh "docker run -d -p 5001:5000 --name flask-app $DOCKER_IMAGE:$IMAGE_TAG"
                // Log container info
                sh "docker ps"
            }
        }
    }
}
