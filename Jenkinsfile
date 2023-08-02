pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                // SonarQube Scanner
                withSonarQubeEnv('SonarQube Server') { //name of Server in config
                    // SonarQube
                    sh 'chmod +x gradlew'
                    sh './gradlew sonarqube'
                }
            }
        }
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
    }
}
