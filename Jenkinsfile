pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'your-credential-id', branch: 'main', url: 'https://github.com/mithunkumar21/sonar.git'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x gradlew'   // Ensure script is executable
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './gradlew sonarqube'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline failed due to Quality Gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
