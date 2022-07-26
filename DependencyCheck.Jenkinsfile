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
    stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./"  
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: '7.1.1'

                dependencyCheckPublisher (
                    pattern: 'dependency-check-report.xml',
                    failedTotalHigh: '0', // fail if any high vulns
                )
            }
        }
    }
}