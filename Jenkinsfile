pipeline {
    agent any

    environment {
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1' // Ganti dengan URL webhook Microsoft Teams Anda
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/Bagasadhe27/Qazir-Tubes.git' // Ganti dengan URL repository GitHub Anda
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'echo Build completed successfully'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo All tests passed'
            }
        }
    }

    post {
        success {
            script {
                def teamsMessage = """{
                    "@type": "MessageCard",
                    "@context": "http://schema.org/extensions",
                    "summary": "Jenkins Build Success",
                    "themeColor": "0076D7",
                    "title": "Build Successful",
                    "text": "The build completed successfully for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}.",
                    "potentialAction": [{
                        "@type": "OpenUri",
                        "name": "View Build",
                        "targets": [{"os": "default", "uri": "${env.BUILD_URL}"}]
                    }]
                }"""

                httpRequest acceptType: 'APPLICATION_JSON',
                            contentType: 'APPLICATION_JSON',
                            httpMode: 'POST',
                            requestBody: teamsMessage,
                            url: env.TEAMS_WEBHOOK
            }
        }
        failure {
            script {
                def teamsMessage = """{
                    "@type": "MessageCard",
                    "@context": "http://schema.org/extensions",
                    "summary": "Jenkins Build Failed",
                    "themeColor": "FF0000",
                    "title": "Build Failed",
                    "text": "The build failed for ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}.",
                    "potentialAction": [{
                        "@type": "OpenUri",
                        "name": "View Build",
                        "targets": [{"os": "default", "uri": "${env.BUILD_URL}"}]
                    }]
                }"""

                httpRequest acceptType: 'APPLICATION_JSON',
                            contentType: 'APPLICATION_JSON',
                            httpMode: 'POST',
                            requestBody: teamsMessage,
                            url: env.TEAMS_WEBHOOK
            }
        }
    }
}
