pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // Gradle Wrapper
                sh 'chmod +x gradlew'
                sh "./gradlew wrapper"                
                // build
                sh "./gradlew build"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // SonarQube Scanner
                withSonarQubeEnv('SonarQube Server') {
                    //  SonarQube 
                    sh 'chmod +x gradlew'
                    sh "./gradlew sonarqube"
                }
            }
        }
    }
}
