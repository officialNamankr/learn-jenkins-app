pipeline{
    agent any
    stages{
        stage('Build'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh'''
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
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install -g serve
                    serve -s build
                    npx playwright test
                    
                    '''
            }
        }
        // stage('Deploy'){
        //     steps{
        //         echo 'Deploying the application'
        //     }
        // }
    }

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}