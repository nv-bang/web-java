pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv('SonarQube Server') {
                        sh 'chmod +x gradlew'    
                       // sh './gradlew wrapper --gradle-version=7.1.1' 
                        sh './gradlew --status'
                        sh './gradlew sonarqube --status'
                    }
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
               }  
            }
        }
    }
}
