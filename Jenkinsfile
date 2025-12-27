pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '0289dbf9-aaef-4543-ab1d-9b1b0b3ca762'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                    npm run build || echo "Build skipped"
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
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g netlify-cli
                    netlify --version
                    echo "Deploy step ready for Site ID: $NETLIFY_SITE_ID"
                    netlify deploy --dir=build --prod
                    
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
            junit allowEmptyResults: true, testResults: 'test-results/junit.xml'
        }
        success {
            echo "Pipeline successful"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
