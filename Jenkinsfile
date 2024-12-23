pipeline {
    agent any
    environment {
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/Bagasadhe27/Qazir-Tubes.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    // Add proper build commands based on your project type
                    if (isUnix()) {
                        sh 'chmod +x ./gradlew'
                        sh './gradlew build -x test'
                    } else {
                        bat 'gradlew build -x test'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    if (isUnix()) {
                        sh './gradlew test'
                    } else {
                        bat 'gradlew test'
                    }
                }
            }
        }
    }
    post {
        success {
            office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                message: "Build Successful: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                status: 'Success'
        }
        failure {
            office365ConnectorSend webhookUrl: env.TEAMS_WEBHOOK,
                message: "Build Failed: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                status: 'Failed'
        }
    }
}