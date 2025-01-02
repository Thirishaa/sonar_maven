pipeline {
    agent any

    tools {
        maven 'sonarmaven' // Ensure this matches the Maven configuration in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Replace with your credentials ID for the SonarQube token
        
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') { // Ensure this matches your SonarQube configuration
                    bat '''
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=sonarmaven \
                        -Dsonar.projectName='sonarmaven' \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=%  SONAR_TOKEN%
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

