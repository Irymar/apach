pipeline {
    agent any
    stages {
        stage('Install Apache2') {
            steps {
                sh '''
                sudo apt update
                sudo apt install -y apache2
                sudo systemctl start apache2
                sudo systemctl enable apache2
                '''
            }
        }
        stage('Check Logs for Errors') {
            steps {
                script {
                    def errorLogs = sh(
                        script: "sudo grep -E ' 4[0-9]{2} | 5[0-9]{2} ' /var/log/apache2/access.log",
                        returnStdout: true
                    )
                    if (errorLogs) {
                        echo "Errors Found in Logs: \\n${errorLogs}"
                    } else {
                        echo "No 4xx or 5xx errors found in logs."
                    }
                }
            }
        }
    }
}
