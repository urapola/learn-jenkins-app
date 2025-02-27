pipeline {
    agent any
    
    environment{
        NETLIFY_SITE_ID ='d19cb347-d155-46d1-b844-6437d2f76479'
    }

    stages {
        stage('Build') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
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
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }

       post{
        always {
            junit 'test-results/junit.xml'
        }
      }
        }

        stage('Deploy') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh'''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to production. site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }
}
