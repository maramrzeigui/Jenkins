pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'oumayma42/test'  // REMPLACEZ par votre username Docker Hub
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Cela utilise déjà votre configuration SCM
            }
        }
        
        stage('Build JAR') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
                    docker.build("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub..."
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").push()
                    }
                    echo "✅ Docker image pushed successfully!"
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
            echo "Docker image: ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
        }
        failure {
            echo '❌ Pipeline failed! Check the logs.'
        }
    }
}
