pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:14-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node -v
                    npm -v
                    npm ci
                    npm run build
                    ls -la

                '''
            }
        }
    }
}
