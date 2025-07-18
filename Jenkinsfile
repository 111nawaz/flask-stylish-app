pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nawaz1114/flask-stylish-app'
        IMAGE_TAG = 'v2'
        DOCKER_CREDENTIALS_ID = 'docker-hub-creds'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '📥 Cloning repository...'
                git branch: 'main', url: 'https://github.com/111nawaz/flask-stylish-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo '🚀 Pushing image to Docker Hub...'
                withDockerRegistry([credentialsId: "$DOCKER_CREDENTIALS_ID", url: ""]) {
                    sh 'docker push $DOCKER_IMAGE:$IMAGE_TAG'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '⚙️ Running Docker container...'
                // Remove old container if it exists
                sh '''
                    docker rm -f flask-app-container || true
                    docker run -d -p 5001:5000 --name flask-app-container $DOCKER_IMAGE:$IMAGE_TAG
                '''
            }
        }
    }
}
