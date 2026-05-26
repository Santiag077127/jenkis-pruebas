pipeline {
    agent any

    environment {
        DISCORD_WEBHOOK = credentials('discord-webhook')
    }

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

        success {

            script {

                discordSend(
                    webhookURL: DISCORD_WEBHOOK,
                    title: "✅ Build Exitoso",
                    description: """
Proyecto: ${env.JOB_NAME}

Build: #${env.BUILD_NUMBER}

Estado: ${currentBuild.currentResult}

URL:
${env.BUILD_URL}
"""
                )
            }
        }

        failure {

            script {

                discordSend(
                    webhookURL: DISCORD_WEBHOOK,
                    title: "❌ Build Fallido",
                    description: """
Proyecto: ${env.JOB_NAME}

Build: #${env.BUILD_NUMBER}

Estado: ${currentBuild.currentResult}

Logs:
${env.BUILD_URL}
"""
                )
            }
        }
    }
}