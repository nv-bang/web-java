pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages {
        stage('sonar quality check'){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
        }
        stage('Build') {
            steps {
                // Gradle Wrapper
                sh 'chmod +x gradlew'
                sh './gradlew wrapper'

                // Build
                sh './gradlew build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // SonarQube Scanner
                withSonarQubeEnv('SonarQube Server') {
                    // SonarQube
                    sh './gradlew sonarqube'
                }
            }
        }
    }
}
