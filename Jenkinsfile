pipeline {
    agent any

    environment {
        IMAGE_NAME = "your-dockerhub-username/weather-app:latest"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Nithya326/Weather_app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t weather-app .
                    docker tag weather-app $IMAGE_NAME
                '''
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}
