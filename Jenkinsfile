pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'nithya0326'
        IMAGE_NAME = "${DOCKERHUB_USER}/weather-app:latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo "📦 Building Docker image..."
                sh 'docker build -t weather-app .'
                sh 'docker tag weather-app $IMAGE_NAME'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "☁️ Pushing image to DockerHub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "🚀 Deploying to Kubernetes..."

                // Debug: show where we are and what files exist
                sh 'echo "Current Directory:" && pwd'
                sh 'echo "Files in workspace:" && ls -la'

                // Apply K8s config
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs above."
        }
    }
}
