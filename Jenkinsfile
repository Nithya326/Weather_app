pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Nithya326/Weather_app.git/'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t weather-app .'
                sh 'docker tag weather-app your-dockerhub-username/weather-app:latest'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push your-dockerhub-username/weather-app:latest'
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
