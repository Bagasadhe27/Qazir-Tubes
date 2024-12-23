pipeline {
    agent {
        docker {
            image 'alpine'
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'echo "Hello from Docker container!"'
            }
        }
    }
}