pipeline {
    agent any

    stages {
        stage('Build'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
                
            }
            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
        stage('Test'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
                
            }
            steps{
            sh '''
            echo "Test stage"
            ls build/index.html && echo "File exists" || echo "File does not exist"
            npm test
            '''
            }
        }
        stage('E2E'){
            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.46.0-noble'
                    reuseNode true
                }
                
            }
            steps{
            sh '''
                npm install -g server
                server -s build
                npx playwright test
            '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
