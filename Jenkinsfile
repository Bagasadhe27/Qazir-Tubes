pipeline {
    agent any

    environment {
        TEAMS_WEBHOOK_URL = "https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1"
        GITHUB_REPO_URL = "https://github.com/Bagasadhe27/Qazir-Tubes.git"
        BRANCH_NAME = "main" // Ganti dengan branch yang Anda gunakan
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from GitHub...'
                // Clone repository dari GitHub
                git branch: "${BRANCH_NAME}", url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Tambahkan perintah build di sini
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Tambahkan perintah test di sini
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                // Tambahkan perintah deploy di sini
            }
        }
    }

    post {
        always {
            script {
                // Kirim notifikasi ke Microsoft Teams
                def buildStatus = currentBuild.result ?: 'SUCCESS'
                def message = """
                {
                    "summary": "Jenkins Build Notification",
                    "sections": [
                        {
                            "activityTitle": "Build Notification",
                            "activitySubtitle": "Status: ${buildStatus}",
                            "facts": [
                                { "name": "Job Name", "value": "${env.JOB_NAME}" },
                                { "name": "Build Number", "value": "${env.BUILD_NUMBER}" },
                                { "name": "Build URL", "value": "${env.BUILD_URL}" },
                                { "name": "GitHub Repository", "value": "${GITHUB_REPO_URL}" },
                                { "name": "Branch", "value": "${BRANCH_NAME}" }
                            ],
                            "markdown": true
                        }
                    ]
                }
                """

                httpRequest(
                    url: env.TEAMS_WEBHOOK_URL,
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: message
                )
            }
        }
    }
}