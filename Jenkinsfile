pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/Bagasadhe27/Qazir-Tubes.git' // Ganti dengan URL repo GitHub
        TEAMS_WEBHOOK_URL = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1' // Ganti dengan URL webhook Teams
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git url: env.GITHUB_REPO
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Tambahkan perintah build Anda di sini
                sh 'echo "Build successful!"'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Tambahkan perintah testing Anda di sini
                sh 'echo "Tests passed!"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                // Tambahkan perintah deployment Anda di sini
                sh 'echo "Deployment complete!"'
            }
        }
    }

    post {
        success {
            echo 'Build and deployment succeeded!'
            sendTeamsNotification('Build and deployment succeeded! :white_check_mark:')
        }
        failure {
            echo 'Build or deployment failed!'
            sendTeamsNotification('Build or deployment failed! :x:')
        }
    }
}

def sendTeamsNotification(String message) {
    httpRequest acceptType: 'APPLICATION_JSON',
                contentType: 'APPLICATION_JSON',
                httpMode: 'POST',
                requestBody: """
                {
                    "text": "${message}"
                }
                """,
                url: env.TEAMS_WEBHOOK_URL
}