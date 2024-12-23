pipeline {
    agent any
    
    environment {
        GITHUB_REPO = 'https://github.com/Bagasadhe27/Qazir-Tubes.git/'
        MS_TEAMS_WEBHOOK = credentials('https://telkomuniversityofficial.webhook.office.com/webhookb2/d6ddeea1-4893-439a-b3a0-a21925537374@90affe0f-c2a3-4108-bb98-6ceb4e94ef15/JenkinsCI/d329faba1d3a4c31ae7b2ed821651636/1fb3b8c7-9026-4a56-ab45-a09a477ff8f8/V2fiPsEwHjaHFI1bpY5v92Qe81w84JvWznwUxFwXN1Krc1')
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
                        url: env.GITHUB_REPO,
                        credentialsId: 'github-creds'
                    ]]
                ])
                
                script {
                    notifyTeams("🔄 Starting CI/CD Pipeline")
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    notifyTeams("🛠 Building Project")
                    sh 'chmod +x ./build.sh'
                    sh './build.sh'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    notifyTeams("🧪 Running Tests")
                    sh 'chmod +x ./test.sh'
                    sh './test.sh'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    notifyTeams("🚀 Deploying to Production")
                    sh 'chmod +x ./deploy.sh'
                    sh './deploy.sh'
                }
            }
        }
    }
    
    post {
        success {
            script {
                notifyTeams("✅ Pipeline Successful!")
            }
        }
        failure {
            script {
                notifyTeams("❌ Pipeline Failed!")
            }
        }
    }
}

def notifyTeams(String message) {
    def payload = """
    {
        "@type": "MessageCard",
        "@context": "http://schema.org/extensions",
        "themeColor": "0076D7",
        "summary": "${message}",
        "sections": [{
            "activityTitle": "${message}",
            "facts": [
                {
                    "name": "Job",
                    "value": "${env.JOB_NAME}"
                },
                {
                    "name": "Build",
                    "value": "${env.BUILD_NUMBER}"
                },
                {
                    "name": "Branch",
                    "value": "${env.GIT_BRANCH}"
                }
            ]
        }]
    }
    """
    
    sh """
        curl -H 'Content-Type: application/json' -d '${payload}' ${MS_TEAMS_WEBHOOK}
    """
}