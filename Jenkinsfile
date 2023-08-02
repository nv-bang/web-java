pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                script{
                    // SonarQube Scanner
                    withSonarQubeEnv('SonarQube Server') { //name of Server in Jenkins config
                        // SonarQube
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                    timeout(time: 30, unit: 'SECONDS') { // config webhook on sonar, if not will failed
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
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
