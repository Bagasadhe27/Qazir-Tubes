pipeline {
    agent any
    
    // Definisi environment variables
    environment {
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1'
        GITHUB_REPO = 'https://github.com/Bagasadhe27/Qazir-Tubes.git/'
    }
    
    // Trigger dari GitHub webhook
    triggers {
        githubPush()
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout code dari GitHub
                git branch: 'main', url: env.GITHUB_REPO
            }
        }
        
        stage('Build') {
            steps {
                // Contoh build step
                sh 'mvn clean install'
            }
            post {
                success {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Build berhasil dilakukan!',
                        status: 'Success',
                        color: '00ff00'
                }
                failure {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Build gagal! Mohon cek Jenkins untuk detailnya.',
                        status: 'Failed',
                        color: 'ff0000'
                }
            }
        }
        
        stage('Test') {
            steps {
                // Contoh test step
                sh 'mvn test'
            }
            post {
                success {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Test berhasil dilakukan!',
                        status: 'Success',
                        color: '00ff00'
                }
                failure {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Test gagal! Mohon cek Jenkins untuk detailnya.',
                        status: 'Failed',
                        color: 'ff0000'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // Contoh deploy step
                sh 'echo "Deploying application..."'
            }
            post {
                success {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Deployment berhasil!',
                        status: 'Success',
                        color: '00ff00'
                }
                failure {
                    office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                        message: 'Deployment gagal! Mohon cek Jenkins untuk detailnya.',
                        status: 'Failed',
                        color: 'ff0000'
                }
            }
        }
    }
    
    // Post actions untuk keseluruhan pipeline
    post {
        always {
            office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                message: "Pipeline selesai - ${currentBuild.currentResult}",
                status: "${currentBuild.currentResult}",
                color: "${currentBuild.currentResult == 'SUCCESS' ? '00ff00' : 'ff0000'}"
        }
    }
}