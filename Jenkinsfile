pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // 下载 Gradle Wrapper
                sh 'chmod +x gradlew'
                sh "./gradlew wrapper"                
                // 构建项目
                sh "./gradlew build"
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // 安装 SonarQube Scanner 插件
                withSonarQubeEnv('SonarQube Server') {
                    // 进行 SonarQube 分析
                    sh 'chmod +x gradlew'
                    sh "./gradlew sonarqube"
                }
            }
        }
    }
}
