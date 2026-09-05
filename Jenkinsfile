pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'randheeer/green-app'
        DOCKER_TAG = '2.0'
        APP_SERVER = '65.0.153.17'
    }

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

                sh '''
                    docker build \
                    -t ${DOCKER_IMAGE}:${DOCKER_TAG} \
                    .
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin

                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy GREEN') {
            steps {

                sshagent(['app-server-ssh']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@${APP_SERVER} \
                        "
                        docker pull ${DOCKER_IMAGE}:${DOCKER_TAG}

                        docker stop green-app || true

                        docker rm green-app || true

                        docker run -d \
			--restart unless-stopped \
                        --name green-app \
                        -p 80:80 \
                        ${DOCKER_IMAGE}:${DOCKER_TAG}
                        "
                    '''
                }
            }
        }

        stage('GREEN Health Check') {
            steps {

                sshagent(['app-server-ssh']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no \
                        ubuntu@${APP_SERVER} \
                        "
                        sleep 5
                        curl -f http://localhost
                        "
                    '''
                }
            }
        }
    }

    post {

        success {
            echo 'GREEN deployment completed successfully'
        }

        failure {
            echo 'GREEN deployment failed'
        }
    }
}
