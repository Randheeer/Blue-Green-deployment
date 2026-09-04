pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Green application'
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying application files'

                sh 'pwd'
                sh 'ls -la'
                sh 'cat index.html'
                sh 'cat Dockerfile'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Green Docker image'

                sh 'docker build -t green-app:2.0 .'
            }
        }

    }

    post {
        success {
            echo 'Green Docker image built successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
