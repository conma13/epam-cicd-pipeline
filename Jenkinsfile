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
                nodejs('node') {
                    sh 'bash ./scripts/build.sh'
                }                
            }
        }
        stage('Test') {
            steps {
                echo 'Mail. Testing...'
                nodejs('node') {
                    sh 'bash ./scripts/test.sh'
                }                
            }
        }
        stage('Build Image') {
            steps {
                echo 'Main. Building image...'
                switch (env.BRANCH_NAME) {
                    case 'main':
                        docker build -t nodemain:v1.0 .
                        break
                    case 'dev':
                        docker build -t nodedev:v1.0 .
                        break
                    default:
                        echo 'Unknown branch'
                        break
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Mail. Deploying...'
            }
        }
    }
}
