pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages {


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
