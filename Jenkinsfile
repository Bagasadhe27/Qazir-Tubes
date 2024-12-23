pipeline {
    agent any

    environment {
        // Power Automate webhook URL (simpan di Jenkins credentials)
        TEAMS_WEBHOOK = credentials('https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1') // ID credentials untuk webhook Power Automate
        PROJECT_NAME = 'Qazir-Tubes'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        // Kirim notifikasi awal build
                        sendTeamsNotification("Build Started", "0000FF")

                        // Proses build
                        sh 'npm install'
                        sh 'npm run build'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sendTeamsNotification("Build Failed", "FF0000")
                        throw e
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        // Kirim notifikasi testing dimulai
                        sendTeamsNotification("Testing Phase", "FFFF00")

                        // Proses testing
                        sh 'npm run test'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sendTeamsNotification("Testing Failed", "FF0000")
                        throw e
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        // Kirim notifikasi deployment dimulai
                        sendTeamsNotification("Deployment Started", "FFA500")

                        // Proses deployment
                        sh 'docker build -t myapp .'
                        sh 'docker tag myapp my-docker-repo/myapp:latest'
                        sh 'docker push my-docker-repo/myapp:latest'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sendTeamsNotification("Deployment Failed", "FF0000")
                        throw e
                    }
                }
            }
        }
    }

    post {
        success {
            sendTeamsNotification("Pipeline Successful", "00FF00")
        }
        failure {
            sendTeamsNotification("Pipeline Failed", "FF0000")
        }
    }
}

// Fungsi untuk mengirim notifikasi ke Teams menggunakan Power Automate Webhook
def sendTeamsNotification(String message, String color) {
    httpRequest(
        url: env.TEAMS_WEBHOOK,
        httpMode: 'POST',
        contentType: 'APPLICATION_JSON',
        requestBody: """{
            "summary": "${env.PROJECT_NAME}",
            "sections": [{
                "activityTitle": "${env.PROJECT_NAME} - ${message}",
                "activitySubtitle": "Build #${env.BUILD_NUMBER}",
                "activityImage": "https://jenkins.io/images/logo-title-opengraph.png",
                "facts": [{
                    "name": "Status",
                    "value": "${message}"
                }, {
                    "name": "Build URL",
                    "value": "${env.BUILD_URL}"
                }],
                "markdown": true
            }],
            "themeColor": "${color}"
        }"""
    )
}
