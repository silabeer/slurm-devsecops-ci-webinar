pipeline {
    agent { label 'linux' }
    tools {
        maven '3.8.6'
    }
    environment {
        scannerHome = tool 'SonarQubeScanner'
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
                    sh "${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=me.aboullaite.dockercon:netty-example \
                -Dsonar.projectName=netty-example \
                -Dsonar.projectVersion=1.1-SNAPSHOT \
                -Dsonar.sources=./src \
                -Dsonar.java.binaries=target/** "
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}