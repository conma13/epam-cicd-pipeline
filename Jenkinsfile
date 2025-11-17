pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [])
            }
        }
        stage('Build') {
            steps {
                echo 'Main. Building...'
                
                sh 'bash ./scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Mail. Testing...'
                sh './scripts/test.sh'
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
