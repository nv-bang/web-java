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
        stage("Docker Build & Docker Push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 10.10.1.195:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 10.10.1.195:8083 
                                docker push  10.10.1.195:8083/springapp:${VERSION}
                                docker rmi 10.10.1.195:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }

    }
//    post {
//		always {
//			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "bang.nguyen@futa.vn";  
//		 }
//	   }
}
