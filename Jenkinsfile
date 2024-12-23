pipeline {
    agent any
    environment {
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Bagasadhe27/Qazir-Tubes.git'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building project..."'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        stage('Post Actions') {
            steps {
                sh 'echo "Post deployment actions..."'
            }
        }
    }
    post {
        success {
            office365ConnectorSend(
                webhookUrl: env.TEAMS_WEBHOOK,
                message: """
                    🟢 Build Successful
                    Job: ${env.JOB_NAME}
                    Build: #${env.BUILD_NUMBER}
                    Duration: ${currentBuild.durationString}
                """.stripIndent(),
                color: '00ff00'
            )
        }
        failure {
            office365ConnectorSend(
                webhookUrl: env.TEAMS_WEBHOOK,
                message: """
                    🔴 Build Failed
                    Job: ${env.JOB_NAME}
                    Build: #${env.BUILD_NUMBER}
                    Duration: ${currentBuild.durationString}
                    Error: ${currentBuild.description ?: 'Check console output'}
                """.stripIndent(),
                color: 'ff0000'
            )
        }
    }
}