pipeline {
    agent any

    environment {
        GITHUB_REPO_URL = 'https://github.com/Bagasadhe27/Qazir-Tubes.git'
        MS_TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1' // Masukkan URL Webhook Microsoft Teams Anda di sini
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: "${GITHUB_REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Tambahkan perintah build proyek Anda di sini, misalnya:
                // sh './gradlew build' (jika menggunakan Gradle)
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Tambahkan perintah untuk menjalankan pengujian
            }
        }

        stage('Notify Microsoft Teams') {
            steps {
                echo 'Sending notification to Microsoft Teams...'
                script {
                    def message = """
                    {
                        "@type": "MessageCard",
                        "@context": "https://schema.org/extensions",
                        "summary": "Build Notification",
                        "themeColor": "0076D7",
                        "sections": [{
                            "activityTitle": "Build Notification",
                            "activitySubtitle": "Jenkins CI/CD Pipeline",
                            "activityImage": "https://cdn-icons-png.flaticon.com/512/2267/2267908.png",
                            "facts": [
                                {"name": "Project:", "value": "Qazir-Tubes"},
                                {"name": "Branch:", "value": "main"},
                                {"name": "Status:", "value": "Success"}
                            ],
                            "markdown": true
                        }]
                    }
                    """
                    httpRequest(
                        acceptType: 'APPLICATION_JSON',
                        contentType: 'APPLICATION_JSON',
                        httpMode: 'POST',
                        requestBody: message,
                        url: "${MS_TEAMS_WEBHOOK}"
                    )
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
    }
}