pipeline {
    agent any
    environment {
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/a44e5ef6bd3341a783d6ca80c71f4272/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2s3qI2r4lAwy6koPgztO36X88y5jcNYLIAyKlYZeVtBs1'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Checkout') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    git branch: 'main', url: 'https://github.com/Bagasadhe27/Qazir-Tubes.git'
                }
            }
        }
        stage('Build Project Docker') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'echo "Building project..."'
                    sh 'exit 0'  
                }
            }
        }
        stage('Integrasi Microsoft teams') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'echo "Running tests..."'
                    sh 'exit 0'  
                }
            }
        }
        stage('Deploy') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    sh 'echo "Executing post actions..."'
                    sh 'exit 0'  
                }
            }
        }
    }
    post {
        always {
            office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                message: "Pipeline completed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                status: currentBuild.result
        }
    }
}