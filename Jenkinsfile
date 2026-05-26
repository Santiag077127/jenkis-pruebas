pipeline {
    agent any

    environment {
        DISCORD_WEBHOOK = credentials('discord-webhook')
    }

    stages {
        stage('Build and Test') {
            steps {
                sh './mvnw clean verify'
            }
        }
    }

    post {

        always {
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }

        success {
            discordSend(
                title: "✅ Build Exitoso",
                description: """
Proyecto: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}

Estado: ${currentBuild.currentResult}

URL:
${env.BUILD_URL}
""",
                webhookURL: DISCORD_WEBHOOK
            )
        }

        failure {
            discordSend(
                title: "❌ Build Fallido",
                description: """
Proyecto: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}

Estado: ${currentBuild.currentResult}

Revisa los logs:
${env.BUILD_URL}
""",
                webhookURL: DISCORD_WEBHOOK
            )
        }
    }
}