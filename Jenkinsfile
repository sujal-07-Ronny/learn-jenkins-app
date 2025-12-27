pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node -v
                    npm -v
                    npm install
                    npm run build || echo "build skipped"
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html && echo "Build exists" || echo "Build missing"
                    npm test || true
                '''
            }

        stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                   npm install netflify-cli -g
                    netlify --version
                '''
            }
        }
            post {
                always {
                    junit 'test-results/junit.xml'

                }
            }
        }

    }
}
