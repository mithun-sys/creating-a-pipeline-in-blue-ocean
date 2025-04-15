pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the Java project...'
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }
    }
}
