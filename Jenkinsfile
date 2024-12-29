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
    }
}
