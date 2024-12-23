pipeline {
    agent any

    environment {
        TEAMS_WEBHOOK_URL = "https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1"
        GITHUB_REPO_URL = "https://github.com/Bagasadhe27/Qazir-Tubes.git" // Ganti sesuai dengan repository Anda
        BRANCH_NAME = "main" // Ganti sesuai dengan branch yang digunakan
    }

    options {
        disableConcurrentBuilds() // Hindari konflik build jika ada build berjalan bersamaan
        timestamps() // Tambahkan timestamp ke output log untuk mempermudah debug
    }

    triggers {
        // Memastikan build berjalan otomatis jika ada perubahan di GitHub
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Pulling code from GitHub...'
                }
                // Clone repository berdasarkan URL dan branch
                git branch: "${BRANCH_NAME}", url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    // Tambahkan perintah build di sini, contoh:
                    // sh 'mvn clean install' (jika menggunakan Maven)
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // Tambahkan perintah untuk menjalankan test di sini, contoh:
                    // sh 'mvn test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the application...'
                    // Tambahkan proses deployment jika diperlukan, contoh:
                    // sh './deploy.sh'
                }
            }
        }
    }

    post {
        success {
            script {
                sendTeamsNotification('SUCCESS')
            }
        }
        failure {
            script {
                sendTeamsNotification('FAILURE')
            }
        }
        unstable {
            script {
                sendTeamsNotification('UNSTABLE')
            }
        }
        always {
            script {
                echo 'Build complete.'
            }
        }
    }
}

def sendTeamsNotification(String buildStatus) {
    def color = buildStatus == 'SUCCESS' ? 'Good' : buildStatus == 'FAILURE' ? 'Attention' : 'Warning'
    def message = """
    {
        "summary": "Jenkins Build Notification",
        "themeColor": "${color}",
        "sections": [
            {
                "activityTitle": "Jenkins Build Notification - ${buildStatus}",
                "activitySubtitle": "Project: Qazir Tubes",
                "facts": [
                    { "name": "Job Name", "value": "${env.JOB_NAME}" },
                    { "name": "Build Number", "value": "${env.BUILD_NUMBER}" },
                    { "name": "GitHub Repository", "value": "${GITHUB_REPO_URL}" },
                    { "name": "Branch", "value": "${BRANCH_NAME}" },
                    { "name": "Build URL", "value": "${env.BUILD_URL}" }
                ],
                "markdown": true
            }
        ]
    }
    """
    // Kirim notifikasi ke Microsoft Teams menggunakan HTTP Request
    httpRequest(
        url: env.TEAMS_WEBHOOK_URL,
        httpMode: 'POST',
        contentType: 'APPLICATION_JSON',
        requestBody: message
    )
}