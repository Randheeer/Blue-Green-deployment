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

        stage('Test SSH Connection') {
            steps {
                sshagent(['app-ec2-ssh']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@YOUR_APP_EC2_PUBLIC_IP \
                        "hostname"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
