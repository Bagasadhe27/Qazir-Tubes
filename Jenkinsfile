pipeline {
    agent any
    
    environment {
        MSTEAMS_WEBHOOK = credentials('https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1')
        GITHUB_REPO = 'https://github.com/Bagasadhe27/Qazir-Tubes.git/'
    }
    
    triggers {
        githubPush()
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: "${GITHUB_REPO}",
                        credentialsId: 'github-credentials'
                    ]]
                ])
                
                office365ConnectorSend message: "🔄 Starting CI/CD pipeline for ${env.JOB_NAME}",
                    status: "Started",
                    webhookUrl: "${MSTEAMS_WEBHOOK}"
            }
        }
        
        stage('Build') {
            steps {
                script {
                    try {
                        sh '''
                            echo "Building application..."
                            # Add your build commands here
                            npm install
                            npm run build
                        '''
                        
                        office365ConnectorSend message: "✅ Build stage successful",
                            status: "Build Success",
                            color: "05b222",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                    } catch (Exception e) {
                        office365ConnectorSend message: "❌ Build stage failed: ${e.getMessage()}",
                            status: "Build Failed",
                            color: "d00000",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                        throw e
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    try {
                        sh '''
                            echo "Running tests..."
                            # Add your test commands here
                            npm test
                        '''
                        
                        office365ConnectorSend message: "✅ Test stage successful",
                            status: "Tests Passed",
                            color: "05b222",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                    } catch (Exception e) {
                        office365ConnectorSend message: "❌ Test stage failed: ${e.getMessage()}",
                            status: "Tests Failed",
                            color: "d00000",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                        throw e
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    try {
                        sh '''
                            echo "Deploying application..."
                            # Add your deployment commands here
                            npm run deploy
                        '''
                        
                        office365ConnectorSend message: "✅ Deployment successful",
                            status: "Deployed",
                            color: "05b222",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                    } catch (Exception e) {
                        office365ConnectorSend message: "❌ Deployment failed: ${e.getMessage()}",
                            status: "Deploy Failed",
                            color: "d00000",
                            webhookUrl: "${MSTEAMS_WEBHOOK}"
                        throw e
                    }
                }
            }
        }
    }
    
    post {
        success {
            office365ConnectorSend message: "✅ Pipeline completed successfully!",
                status: "Success",
                color: "05b222",
                webhookUrl: "${MSTEAMS_WEBHOOK}"
        }
        failure {
            office365ConnectorSend message: "❌ Pipeline failed! Check Jenkins console for details.",
                status: "Failed",
                color: "d00000",
                webhookUrl: "${MSTEAMS_WEBHOOK}"
        }
    }
}