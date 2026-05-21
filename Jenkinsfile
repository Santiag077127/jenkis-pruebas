pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                sh 'mvn clean verify'
            }
        }
    }

    post {
        always {
            junit testResults: 'target/surefire-reports/*.xml',
                   allowEmptyResults: true
        }
    }
}