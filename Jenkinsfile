pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Main. Checking out...'
                sh 'git branch -a'
                sh 'ls -al'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [])
                echo 'Main. Debug after Check out...'
                sh 'git branch -a'
                sh 'ls -al'
                echo 'Main. Checked out...'
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
