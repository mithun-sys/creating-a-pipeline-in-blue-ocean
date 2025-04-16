pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mithunsys/springboot-app'  // change to your Docker Hub repo name
    }

    stages {
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
                        dockerImage = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Archiving the built JAR file...'
            archiveArtifacts artifacts: 'java-app/target/*.jar', fingerprint: true
        }
    }
}
