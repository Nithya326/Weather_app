pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Set this ID in Jenkins credentials
    DOCKER_IMAGE = "${DOCKERHUB_CREDENTIALS_USR}/weather-app:latest"
  }

  stages {
    stage('Clone') {
      steps {
        git branch: 'main', url: 'https://github.com/Nithya326/Weather_app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t weather-app .
          docker tag weather-app $DOCKER_IMAGE
        '''
      }
    }

    stage('Push to DockerHub') {
      steps {
        sh '''
          echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
          docker push $DOCKER_IMAGE
        '''
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
          kubectl apply -f k8s/deployment.yaml
          kubectl apply -f k8s/service.yaml
        '''
      }
    }
  }
}
