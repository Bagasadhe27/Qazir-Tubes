pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/Bagasadhe27/Qazir-Tubes.git' // Ganti dengan URL repository GitHub Anda
        DOCKER_IMAGE = 'qazir-docker' // Ganti dengan nama image Docker Anda
        TEAMS_WEBHOOK = 'https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1' // Ganti dengan URL webhook Microsoft Teams Anda
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub'
                git branch: 'main', url: env.GITHUB_REPO
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to registry'
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image(env.DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Notify Teams') {
            steps {
                echo 'Sending notification to Microsoft Teams'
                script {
                    def teamsMessage = "{" +
                        "\"@type\": \"MessageCard\"," +
                        "\"@context\": \"http://schema.org/extensions\"," +
                        "\"summary\": \"Jenkins Build Notification\"," +
                        "\"themeColor\": \"0076D7\"," +
                        "\"title\": \"Jenkins Pipeline Notification\"," +
                        "\"text\": \"Build and Docker image push completed successfully!\"" +
                    "}"

                    httpRequest acceptType: 'APPLICATION_JSON',
                               contentType: 'APPLICATION_JSON',
                               httpMode: 'POST',
                               requestBody: teamsMessage,
                               url: env.TEAMS_WEBHOOK
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        failure {
            script {
                def failureMessage = "{" +
                    "\"@type\": \"MessageCard\"," +
                    "\"@context\": \"http://schema.org/extensions\"," +
                    "\"summary\": \"Jenkins Build Failure\"," +
                    "\"themeColor\": \"FF0000\"," +
                    "\"title\": \"Build Failed\"," +
                    "\"text\": \"The pipeline failed at some stage.\"" +
                "}"

                httpRequest acceptType: 'APPLICATION_JSON',
                           contentType: 'APPLICATION_JSON',
                           httpMode: 'POST',
                           requestBody: failureMessage,
                           url: env.TEAMS_WEBHOOK
            }
        }
    }
}
