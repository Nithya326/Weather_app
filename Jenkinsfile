pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Add in Jenkins
  }

  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/Nithya326/Weather_app.git'
      }
    }

    stage('Build & Push Docker Image') {
      steps {
        sh '''
          docker build -t weather-app .
          docker tag weather-app $DOCKERHUB_CREDENTIALS_USR/weather-app:latest
          echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
          docker push $DOCKERHUB_CREDENTIALS_USR/weather-app:latest
        '''
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
        sh 'kubectl apply -f k8s/service.yaml'
      }
    }
  }
}
