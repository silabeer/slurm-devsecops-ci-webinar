pipeline {
    agent { label 'linux' }
    tools {
        maven '3.8.6'
    }
    stages {
        stage('Check Git') {
            steps {
                git branch: 'master', url: 'https://github.com/aboullaite/java-docker.git'
            }
        }
        stage('Scan') {
            steps {
                withSonarQubeEnv(installationName: 'sq') {
                    sh 'mvn clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar -Dsonar.java.binaries=target/**'
                }
            }
        }
    }
}