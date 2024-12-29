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
                    // Перевірка, чи існує файл логів
                    def logExists = sh(
                        script: "test -f /var/log/apache2/access.log && echo 'exists' || echo 'not found'",
                        returnStdout: true
                    ).trim()

                    if (logExists == 'exists') {
                        // Читання логів та пошук помилок
                        def errorLogs = sh(
                            script: "sudo grep -E ' 4[0-9]{2} | 5[0-9]{2} ' /var/log/apache2/access.log || true",
                            returnStdout: true
                        ).trim()

                        if (errorLogs) {
                            echo "Errors Found in Logs: \\n${errorLogs}"
                        } else {
                            echo "No 4xx or 5xx errors found in logs."
                        }
                    } else {
                        echo "Log file not found. Skipping error check."
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deployment step executed'
                sh '''
                # Тестовий запит до Apache2, щоб згенерувати логи
                curl -I http://localhost
                '''
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution complete.'
        }
    }
}
