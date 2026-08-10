pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Jenkins! Pipeline is working.'
            }
        }
        stage('Checkout Confirmation') {
            steps {
                echo 'Repo successfully checked out via Jenkins.'
                sh 'ls -la'
            }
        }
        stage('System Info') {
            steps {
                sh 'whoami'
                sh 'date'
            }
        }
    }

    post {
        success {
            echo 'Test pipeline completed successfully!'
        }
    }
}
