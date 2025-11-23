#!/usr/bin/env groovy
pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [])
                script {
                    switch (env.BRANCH_NAME) {
                        case 'main':
                            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [])
                            break
                        case 'dev':
                            checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [])
                            break
                        default:
                            echo 'Checking out. Unknown branch. Nothing to do'
                            break
                    }
                }
                echo 'Checked out successfully'
            }
        }
        stage('Build') {
            steps {
                nodejs('node') {
                    sh 'bash ./scripts/build.sh'
                }                
                echo 'Built successfully'
            }
        }
        stage('Test') {
            steps {
                nodejs('node') {
                    sh 'bash ./scripts/test.sh'
                }                
                echo 'Tested successfully'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    switch (env.BRANCH_NAME) {
                        case 'main':
                            sh 'docker -H tcp://docker:2375 build -t nodemain:v1.0 .'
                            break
                        case 'dev':
                            sh 'docker -H tcp://docker:2375 build -t nodedev:v1.0 .'
                            break
                        default:
                            echo 'Building image. Unknown branch. Nothing to do'
                            break
                    }
                }
                echo 'Image built successfully'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    switch (env.BRANCH_NAME) {
                        case 'main':
                            sh 'docker -H tcp://docker:2375 rm -f nodemain || true'
                            sh 'docker -H tcp://docker:2375 run -d --name nodemain --expose 3000 -p 3000:3000 nodemain:v1.0'
                            break
                        case 'dev':
                            sh 'docker -H tcp://docker:2375 rm -f nodedev || true'
                            sh 'docker -H tcp://docker:2375 run -d --name nodedev --expose 3001 -p 3001:3000 nodedev:v1.0'
                            break
                        default:
                            echo 'Deploying. Unknown branch. Nothing to do'
                            break
                    }
                }
                echo 'Deployed successfully'
            }
        }
    }
}
