pipeline {
    agent any
    
    environment {
        // Teams webhook URL (simpan sebagai credential di Jenkins)
        TEAMS_WEBHOOK = credentials('https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1')
        // Informasi Project
        PROJECT_NAME = 'Qazir-Tubes'
        BUILD_CAUSE = currentBuild.getBuildCauses().toString()
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    try {
                        // Notifikasi start build
                        office365ConnectorSend webhookUrl: "${TEAMS_WEBHOOK}",
                            message: "${PROJECT_NAME} - Build Started",
                            status: "Started",
                            color: "0000FF",
                            factDefinitions: [
                                [name: "Build Number", template: "${BUILD_NUMBER}"],
                                [name: "Started By", template: "${BUILD_CAUSE}"]
                            ]
                            
                        // Proses build disini
                        sh 'npm install'
                        sh 'npm run build'
                        
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    try {
                        // Notifikasi testing dimulai
                        office365ConnectorSend webhookUrl: "${TEAMS_WEBHOOK}",
                            message: "${PROJECT_NAME} - Testing Phase",
                            status: "Testing",
                            color: "FFFF00"
                            
                        // Proses testing
                        sh 'npm run test'
                        
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    try {
                        // Notifikasi deployment dimulai
                        office365ConnectorSend webhookUrl: "${TEAMS_WEBHOOK}",
                            message: "${PROJECT_NAME} - Deployment Started",
                            status: "Deploying",
                            color: "FFA500"
                            
                        // Proses deployment
                        sh 'docker build -t myapp .'
                        sh 'docker push myapp:latest'
                        
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }
    
    post {
        success {
            office365ConnectorSend webhookUrl: "${TEAMS_WEBHOOK}",
                message: "${PROJECT_NAME} - Pipeline Successful",
                status: "Success",
                color: "00FF00",
                factDefinitions: [
                    [name: "Build Duration", template: "${currentBuild.durationString}"],
                    [name: "Build Number", template: "${BUILD_NUMBER}"]
                ]
        }
        
        failure {
            office365ConnectorSend webhookUrl: "${TEAMS_WEBHOOK}",
                message: "${PROJECT_NAME} - Pipeline Failed",
                status: "Failed",
                color: "FF0000",
                factDefinitions: [
                    [name: "Failed Stage", template: "${STAGE_NAME}"],
                    [name: "Error Message", template: "${currentBuild.description}"],
                    [name: "Build Duration", template: "${currentBuild.durationString}"]
                ]
        }
    }
}