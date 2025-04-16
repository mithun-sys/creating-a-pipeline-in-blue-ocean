pipeline {
  agent any

  environment {
    IMAGE_NAME = 'mithunsys/springboot-app'
    IMAGE_TAG = 'latest'
  }

  stages {

    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        echo 'Compiling and packaging the Spring Boot app...'
        dir('java-app') {
          sh 'mvn clean install'
        }
      }
    }

    stage('Test') {
      steps {
        echo 'Running unit tests...'
        dir('java-app') {
          sh 'mvn test'
        }
      }
    }

    stage('Docker Build & Push') {
      steps {
        script {
          dir('java-app') {
            withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
              sh """
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                docker push ${IMAGE_NAME}:${IMAGE_TAG}
              """
            }
          }
        }
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully!'
    }
    failure {
      echo '❌ Pipeline failed. Please check the logs.'
    }
  }
}
