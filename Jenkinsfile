pipeline {
    agent any

    stages {
        /*
         stage('Build') {
             agent {
                 docker {
                     image 'node:18-alpine'
                     reuseNode true
                 }
             }
             steps {
                 sh '''
                 ls -al
                 node --version 
                 npm --version
                 npm ci 
                 npm run build 
                 ls -al
                 '''
             }
         }
        */
        stage('Run Test') {
            parallel {
                stage('Test') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        #way to comment in script/ bash/shell
                        #echo "Test stage"
                        test -f  build/index.html
                        npm test
                        '''
                    }
                }
                stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        npm install serve
                        node_modules/.bin/serve -s build &
                        npx playwrite test
                        '''
                    }
                }

            }
        }

    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
// added build stage
// starting end to end testing stage
