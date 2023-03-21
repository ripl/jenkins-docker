// Requires that the Docker Pipeline plugin be installed
pipeline {
  agent any
  environment {
    // Common variables
    RELEASE_VERSION = "1.0.0"
    
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')

    // Tag: latest
    BASE_IMAGE_LATEST = "jenkins/jenkins:lts"
    BUILD_IMAGE_LATEST = "ripl/jenkins:latest"
  }
  stages {
    stage('Update Base Image') {
      steps {
        sh 'docker pull $BASE_IMAGE_LATEST'
      }
    }
    stage('Build Image') {
      steps {
        sh 'docker build -t $BUILD_IMAGE_LATEST -f Dockerfile ./'
      }
    }
    stage('Push Image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
          sh 'docker push $BUILD_IMAGE_LATEST'
        }
      }
    }
    stage('Clean up') {
      steps {
        sh 'docker rmi $BUILD_IMAGE_LATEST'
        sh 'docker rmi $BASE_IMAGE_LATEST'

        cleanWs()
      }
    }
  }
}
