pipeline {
    agent any

    environment {
        GRADLE_USER_HOME = "${pwd()}/.gradle"
    }

    stages {
        stage("sonar quality check"){
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
