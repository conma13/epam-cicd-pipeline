#!/usr/bin/env groovy
pipeline {
    agent {
        docker {
            image 'docker'
        }
    }

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
                echo 'Main. Testing...'
                nodejs('node') {
                    sh 'bash ./scripts/test.sh'
                }                
            }
        }
        stage('Build Image') {
            steps {
                echo 'Main. Building image...'
                sh 'cat Jenkinsfile'
                script {
                    switch (env.BRANCH_NAME) {
                        case 'main':
                            def myImage = docker.build 'nodemain:v1.0'
                            break
                        case 'dev':
                            def myImage = docker.build 'nodedev:v1.0'
                            break
                        default:
                            echo 'Unknown branch'
                            break
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Main. Deploying...'
            }
        }
    }
}
