#!/usr/bin/env groovy
pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/${BRANCH_NAME}']], extensions: [], userRemoteConfigs: [])
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
                sh 'docker -H tcp://docker:2375 build -t node${BRANCH_NAME}:v1.0 .'
                echo 'Image built successfully'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def port = ''
                    switch (env.BRANCH_NAME) {
                        case "main":
                            port = '3000'
                            break
                        case "dev":
                            port = '3001'
                            break
                        default:
                            port = '3000'
                            break
                    }
                    sh '''
                      echo "port=" ${port}
                      filter = "publish=${port}"
                      echo "filter=" ${filter}
                      CONTAINERS=$(docker -H tcp://docker:2375 ps --filter "${filter}" -q)
                      if [ -n "$CONTAINERS" ]; then
                        echo "Stop containers on ${port} port: ${CONTAINERS}"
                        docker -H tcp://docker:2375 rm -f ${CONTAINERS}
                      else
                        echo "No containers on port ${port}"
                      fi
                      docker -H tcp://docker:2375 \
                        run -d --name node${BRANCH_NAME} \
                        --expose ${port} -p ${port}:3000 \
                        node${BRANCH_NAME}:v1.0
                    '''
                }
                echo 'Deployed successfully'
            }
        }
    }
}
