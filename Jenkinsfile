// Jenkinsfile for CI/CD Pipeline with Microsoft Teams Notifications
pipeline {
    agent any

    environment {
        // Replace with your Microsoft Teams webhook URL
        MS_TEAMS_WEBHOOK = credentials('https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1') 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Replace this with your actual build command
                sh 'npm install && npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Replace this with your actual test command
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Replace this with your actual deployment command
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
            sendTeamsNotification('SUCCESS', "Pipeline completed successfully.")
        }

        failure {
            echo 'Pipeline failed.'
            sendTeamsNotification('FAILURE', "Pipeline failed.")
        }

        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}

// Function to send Microsoft Teams notifications
def sendTeamsNotification(status, message) {
    def color = status == 'SUCCESS' ? '00FF00' : 'FF0000'
    def payload = [
        '@type'       : 'MessageCard',
        '@context'    : 'http://schema.org/extensions',
        'themeColor'  : color,
        'summary'     : "Jenkins Pipeline ${status}",
        'sections'    : [[
            'activityTitle' : "Jenkins Pipeline Notification",
            'activitySubtitle': "Status: ${status}",
            'text'           : message
        ]]
    ]
    
    httpRequest(
        url: MS_TEAMS_WEBHOOK,
        httpMode: 'POST',
        contentType: 'APPLICATION_JSON',
        requestBody: groovy.json.JsonOutput.toJson(payload)
    )
}