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
    }

    post {
        success {
            echo 'Green application verification successful'
        }

        failure {
            echo 'Green application verification failed'
        }
    }
}
