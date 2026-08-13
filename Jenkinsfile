pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-creds')
        IMAGE_NAME = 'shubhampatel6395/portfolio'
        EC2_HOST = '13.207.191.122'
        CONTAINER_NAME = 'portfolio-container'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin
                    docker push $IMAGE_NAME:latest
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@$EC2_HOST "
                            docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW &&
                            docker pull $IMAGE_NAME:latest &&
                            docker rm -f $CONTAINER_NAME || true &&
                            docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME:latest
                        "
                    '''
                }
            }
        }

    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed - check logs.'
        }
    }
}
