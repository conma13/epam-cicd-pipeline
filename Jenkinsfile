pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Main. Checking out...'
                script {
                    git branch -a
                    ls -al
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Main. Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Mail. Testing...'
            }
        }
        stage('Build Image') {
            steps {
                echo 'Main. Building image...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Mail. Deploying...'
            }
        }
    }
}
