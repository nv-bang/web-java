pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
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
                    sh 'chmod +x gradlew'
                    sh './gradlew sonarqube'
                }
            }
        }
    }
}
