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
                            sh 'cp src/main.svg src/logo.svg'
                            docker.withServer('tcp://docker:2375') {
                                def myImage = docker.build 'nodemain:v1.0'
                            }
                            break
                        case 'dev':
                            sh 'cp src/dev.svg src/logo.svg'
                            docker.withServer('tcp://docker:2375') {
                                def myImage = docker.build 'nodedev:v1.0'
                            }
                            break
                        default:
                            echo 'Unknown branch. Nothing to do'
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
                            docker.withServer('tcp://docker:2375') {
                                docker.image('nodedev:v1.0').run('--name nodedev --expose 3000 -p 3001:3000')
                            }
                            break
                        default:
                            echo 'Unknown branch. Nothing to do'
                            break
                    }
                }
                echo 'Deployed successfully'
            }
        }
    }
}
