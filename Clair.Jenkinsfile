pipeline {
    agent { label 'linux' }
    tools {
        maven '3.8.6'
    }
    environment {
        CLAIR_SERVER = "194.67.91.216"
    }
    stages {
        stage('Check Git') {
            steps {
                git branch: 'master', url: 'https://github.com/aboullaite/java-docker.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Configure ClairScanner') {
            steps {
                sh """
                    wget https://github.com/arminc/clair-scanner/releases/download/v12/clair-scanner_linux_amd64
                    mv clair-scanner_linux_amd64 clair-scanner &&  chmod +x clair-scanner     
                    """
            }
        }
        stage('Finding Vulnerabilites') {
            steps {
                sh """
                docker build -t simple:${BUILD_NUMBER} .
                ./clair-scanner --ip ${CLAIR_SERVER}  -r report.json simple:1 || exit 0
                 """
            }
        }
        stage('Converting JSON into HTML ') {
            steps {
                sh """
         cat <<EOF > JSON_TO_HTML.py
import json
from json2html import *
with open('report.json') as f:
    d = json.load(f)
    scanOutput = json2html.convert(json=d)
    htmlReportFile = "report.html"
    with open(htmlReportFile, 'w') as htmlfile:
        htmlfile.write(str(scanOutput))
        print("Json file is converted into html")
EOF
                """
                sh """
                    python3 JSON_TO_HTML.py
                """
                publishHTML target: [
                        allowMissing         : false,
                        alwaysLinkToLastBuild: false,
                        keepAll              : true,
                        reportDir            : './',
                        reportFiles          : 'report.html',
                        reportName           : 'ClairScanner Report'
                ]
            }
        }
    }
}