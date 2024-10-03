pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                //mehma ''' daala thiynne me wage udrutha thunak athule apita one thrm sh command liynna puluwan
                sh '''          
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    ng build
                '''
                //The npm ci command is used to install Node.js project dependencies in a clean and reliable way. It is similar to npm install, but with a few important differences that make it particularly useful for CI (Continuous Integration) environments like Jenkins.
            }
        }
    }
}
